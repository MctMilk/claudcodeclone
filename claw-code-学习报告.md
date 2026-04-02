# claw-code 仓库学习分析报告

## 一、总体架构概览

claw-code 是 Claude Code 的开源实现（部分），采用 **Python + Rust 混合架构**：

```
┌─────────────────────────────────────────────────────────────┐
│                      Python 层（Porting Workspace）          │
│  src/main.py · query_engine.py · tool_pool.py · runtime.py  │
│  主要职责：CLI 入口、查询路由、会话管理、工具池编排           │
├─────────────────────────────────────────────────────────────┤
│                      TypeScript Archive（参考数据）          │
│  src/reference_data/subsystems/*.json                       │
│  存储原始 TypeScript 代码快照，作为 Python 移植的参考        │
├─────────────────────────────────────────────────────────────┤
│                      Rust 后端（核心逻辑）                   │
│  runtime · plugins · api · lsp · server · claw-cli · tools │
│  主要职责：会话管理、工具执行、插件系统、MCP、远程会话       │
└─────────────────────────────────────────────────────────────┘
```

## 二、代码结构分析

### 2.1 Rust 后端结构 (`rust/crates/`)

| Crate | 职责 |
|--------|------|
| `runtime` | 核心运行时：会话管理、对话循环、工具执行、权限控制、Hook 系统、上下文压缩 |
| `plugins` | 插件系统：Builtin/Bundled/External 三类插件，生命周期管理，Hook 聚合 |
| `api` | API 客户端：OpenAI 兼容接口、SSE 流式传输 |
| `lsp` | Language Server Protocol 集成 |
| `server` | Axum Web 服务器，多会话管理，SSE 广播 |
| `tools` | 工具 crate |
| `claw-cli` | 命令行入口 |

### 2.2 Python 层结构 (`src/`)

| 模块 | 职责 |
|------|------|
| `main.py` | CLI 入口，支持 20+ 子命令 |
| `runtime.py` | 运行时封装：Prompt 路由、Turn 循环、会话持久化 |
| `query_engine.py` | 查询引擎：多轮对话管理、预算控制、Transcript 管理 |
| `tool_pool.py` | 工具池组装 |
| `commands.py` | 命令路由 |
| `session_store.py` | 会话持久化（JSON 文件） |
| `permissions.py` | 权限上下文过滤 |
| `models.py` | 共享数据类 |

## 三、核心设计模式分析

### 3.1 工具编排（Tool Orchestration）

**Rust 端 `ConversationRuntime<C, T>` 设计：**

```rust
pub struct ConversationRuntime<C, T> {
    session: Session,
    api_client: C,           // ApiClient trait - 抽象 API 调用
    tool_executor: T,         // ToolExecutor trait - 抽象工具执行
    permission_policy: PermissionPolicy,
    system_prompt: Vec<String>,
    max_iterations: usize,
    usage_tracker: UsageTracker,
    hook_runner: HookRunner,
}
```

**关键设计亮点：**

1. **双 Trait 抽象**：`ApiClient` 和 `ToolExecutor` 都是 trait，支持任意实现替换
2. **标准 Agent 循环**：run_turn 方法实现经典的 ReAct 模式
   - 用户输入 → API 请求 → 流式事件 → 提取工具调用 → 权限检查 → 工具执行 → Hook 处理 → 结果记录
3. **权限策略模式**：`PermissionPolicy` 可插拔，支持 `Allow/Deny` 决策
4. **Hook 系统**：PreToolUse 和 PostToolUse 两级 Hook，支持 shell 命令作为 hook

**Python 端 `PortRuntime` 设计：**

```python
class PortRuntime:
    def route_prompt(self, prompt: str, limit: int = 5) -> list[RoutedMatch]:
        # 基于 token 匹配的 prompt 路由
        # 支持 command 和 tool 两种类型

    def run_turn_loop(self, prompt: str, limit: int = 5, max_turns: int = 3) -> list[TurnResult]:
        # 多轮对话循环
        # 支持 structured_output
```

### 3.2 会话管理（Session Management）

**Rust Session 模型：**

```rust
pub struct Session {
    pub version: u32,
    pub messages: Vec<ConversationMessage>,
}

pub struct ConversationMessage {
    pub role: MessageRole,        // System/User/Assistant/Tool
    pub blocks: Vec<ContentBlock>, // Text/ToolUse/ToolResult
    pub usage: Option<TokenUsage>,
}
```

**设计亮点：**

1. **版本化设计**：`version: u32` 字段支持未来格式迁移
2. **Block-based 消息**：`ContentBlock` 枚举支持 Text、ToolUse、ToolResult
3. **JSON 序列化**：完整实现了 `to_json()` / `from_json()`
4. **Python 镜像**：`session_store.py` 提供 `StoredSession` 等价物
5. **Token 追踪**：`TokenUsage` 记录 input_tokens、output_tokens、cache 统计

**Python 会话持久化：**

```python
@dataclass(frozen=True)
class StoredSession:
    session_id: str
    messages: tuple[str, ...]
    input_tokens: int
    output_tokens: int
# 保存到 .port_sessions/{session_id}.json
```

### 3.3 上下文压缩（Context Compaction）

**`compact.rs` 实现：**

```rust
pub struct CompactionConfig {
    pub preserve_recent_messages: usize,  // 默认保留 4 条
    pub max_estimated_tokens: usize,       // 默认 10,000 tokens
}

pub fn should_compact(session: &Session, config: CompactionConfig) -> bool
pub fn compact_session(session: &Session, config: CompactionConfig) -> CompactionResult
```

**设计亮点：**

1. **双重压缩触发**：消息数量 + token 数量双重检查
2. **摘要生成**：从历史消息生成摘要
3. **恢复指令**：`get_compact_continuation_message()` 生成 "Continue directly" 指令
4. **标签提取**：支持 `<analysis>` 和 `<summary>` 标签块提取

### 3.4 插件系统（Plugin System）

**三类插件：**

```rust
pub enum PluginKind {
    Builtin,   // 内置插件
    Bundled,   // 捆绑插件（随仓库发布）
    External,  // 外部插件（用户安装）
}
```

**插件清单（`plugin.json`）：**

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "permissions": ["read", "write", "execute"],
  "defaultEnabled": false,
  "hooks": {
    "PreToolUse": ["./hooks/pre.sh"],
    "PostToolUse": ["./hooks/post.sh"]
  },
  "lifecycle": {
    "Init": ["./lifecycle/init.sh"],
    "Shutdown": ["./hooks/shutdown.sh"]
  },
  "tools": [{
    "name": "my_tool",
    "description": "A custom tool",
    "inputSchema": { "type": "object" },
    "command": "./tools/my_tool.sh",
    "requiredPermission": "workspace-write"
  }]
}
```

**设计亮点：**

1. **权限分级**：`read-only`、`workspace-write`、`danger-full-access`
2. **Hook 聚合：`PluginRegistry::aggregated_hooks()` 合并所有启用插件的 Hook
3. **工具聚合：`PluginRegistry::aggregated_tools()` 去重合并所有工具
4. **生命周期管理：`Init/Shutdown` 钩子自动执行
5. **自动安装：bundled 插件自动安装到 registry

### 3.5 多会话服务器（Multi-Session Server）

**Axum Web 服务器设计：**

```rust
pub struct AppState {
    sessions: SessionStore,           // Arc<RwLock<HashMap<SessionId, Session>>>
    next_session_id: Arc<AtomicU64>,
}

pub struct Session {
    pub id: SessionId,
    pub created_at: u64,
    pub conversation: RuntimeSession,
    events: broadcast::Sender<SessionEvent>,  // SSE 广播
}
```

**设计亮点：**

1. **多会话并发**：每个会话独立 `RuntimeSession`
2. **SSE 广播**：基于 `broadcast::channel` 的实时推送
3. **Async/Await**：全异步设计，使用 `async-stream`

### 3.6 权限系统

**Rust 端：**

```rust
pub enum PermissionPolicy {
    AlwaysAllow,
    AlwaysDeny,
    Prompt,  // 交互式询问
}
```

**Python 端：**

```python
@dataclass(frozen=True)
class ToolPermissionContext:
    deny_names: frozenset[str]
    deny_prefixes: tuple[str, ...]

    def blocks(self, tool_name: str) -> bool:
        lowered = tool_name.lower()
        return lowered in self.deny_names or any(lowered.startswith(prefix) for prefix in self.deny_prefixes)
```

**设计亮点：**

1. **前缀匹配**：支持 `deny_prefixes` 如 `mcp_` 拒绝所有 MCP 工具
2. **大小写不敏感**：`tool_name.lower()` 统一处理

### 3.7 MCP（Model Context Protocol）支持

**相关文件：**

- `runtime/src/mcp.rs` - MCP 协议处理
- `runtime/src/mcp_client.rs` - MCP 客户端传输
- `runtime/src/mcp_stdio.rs` - STDIO 进程管理

**设计亮点：**

1. **多传输支持**：Stdio、WebSocket、Managed Proxy
2. **OAuth 支持：`oauth.rs` 实现 PKCE 授权码流程
3. **工具名称规范化：`normalize_name_for_mcp()`

## 四、代码风格和规范亮点

### 4.1 Rust 代码风格

1. **`#[must_use]` 注解**：标注有副作用的函数必须使用返回值
2. **错误处理**：自定义错误枚举，完整实现 `Display` 和 `Error`
3. **模块化设计**：每个子模块单独文件，通过 `mod.rs` 聚合
4. **泛型 + Trait Bound**：`ConversationRuntime<C, T> where C: ApiClient, T: ToolExecutor`
5. **测试覆盖**：每个模块都有完整的单元测试

### 4.2 Python 代码风格

1. **Frozen dataclass**：所有模型类使用 `@dataclass(frozen=True)` 不可变设计
2. **类型提示完整**：所有函数有完整的类型注解
3. **Porting Workspace 模式**：Python 作为 TypeScript 的参考实现，便于移植
4. **Reference Data 分离：TS 代码快照存储为 JSON，Python 通过 `reference_data/` 目录读取

### 4.3 配置管理

**Rust 配置系统（`runtime/src/config.rs`）：**

```rust
pub struct RuntimeConfig {
    pub permission_mode: ResolvedPermissionMode,
    pub hooks: RuntimeHookConfig,
    pub features: RuntimeFeatureConfig,
    pub mcp: McpConfigCollection,
}
```

**设计亮点：**

1. **配置来源优先级**：默认值 → 环境变量 → 配置文件
2. **Schema 验证：ClawSettings JSON Schema 校验
3. **MCP 配置集合：支持多个 MCP 服务器配置

## 五、值得借鉴的设计

### 5.1 工具编排设计

```
┌──────────────────────────────────────────────────────────────┐
│                     User Input                               │
└─────────────────────┬────────────────────────────────────────┘
                      ▼
┌──────────────────────────────────────────────────────────────┐
│              ConversationRuntime.run_turn()                   │
│  1. Build ApiRequest (system_prompt + messages)              │
│  2. api_client.stream() → Vec<AssistantEvent>               │
│  3. build_assistant_message() → ConversationMessage         │
│  4. Extract pending_tool_uses                                │
└─────────────────────┬────────────────────────────────────────┘
                      ▼
┌──────────────────────────────────────────────────────────────┐
│              Tool Execution Loop                             │
│  For each tool:                                              │
│    ├── permission_policy.authorize() → Allow/Deny            │
│    ├── hook_runner.run_pre_tool_use() → HookRunResult       │
│    ├── tool_executor.execute() → output                      │
│    ├── hook_runner.run_post_tool_use() → HookRunResult       │
│    └── record tool_result message                            │
└─────────────────────┬────────────────────────────────────────┘
                      ▼
┌──────────────────────────────────────────────────────────────┐
│              Continue until no pending tools                 │
└──────────────────────────────────────────────────────────────┘
```

### 5.2 会话持久化流程

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────────┐
│ TurnResult  │────▶│ TranscriptStore │────▶│ StoredSession (JSON) │
│             │     │ (in-memory)   │     │ .port_sessions/     │
└─────────────┘     └──────────────┘     └─────────────────────┘
```

### 5.3 插件 Hook 执行流程

```
Tool Execution
      │
      ▼
┌─────────────────┐
│ PreToolUse Hook │
│ (shell command) │
└────────┬────────┘
         │ Exit Code
         ├── 0: Allow + capture stdout as message
         ├── 2: Deny + abort tool execution
         └── Other: Warn + continue
         │
         ▼
┌─────────────────┐
│ tool_executor   │
│  .execute()     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│PostToolUse Hook │
└────────┬────────┘
         │
         ▼
   Tool Result
```

## 六、可改进之处

1. **Python/Rust 职责边界**：Python 端大量 `__init__.py` 只是快照占位符，逻辑在 TypeScript Archive 中，真正的 Python 移植工作未完成
2. **错误恢复机制**：会话压缩后如果 API 调用失败，没有重试和恢复机制
3. **插件隔离**：外部插件运行在同一个进程，通过 shell 命令执行，缺乏真正的沙箱隔离
4. **测试覆盖**：虽然每个 Rust 模块都有测试，但集成测试较少
5. **配置热更新**：运行时配置变更需要重启服务

## 七、总结

claw-code 是一个 **架构设计优秀** 的 Claude Code 开源实现，其核心亮点：

| 维度 | 评价 |
|------|------|
| **工具编排** | ⭐⭐⭐⭐⭐ ReAct 循环 + Hook 系统 + 权限策略，设计完整 |
| **会话管理** | ⭐⭐⭐⭐⭐ 版本化设计 + 上下文压缩 + 完整序列化 |
| **插件系统** | ⭐⭐⭐⭐⭐ 三类插件 + 生命周期管理 + 工具/Hook 聚合 |
| **代码质量** | ⭐⭐⭐⭐ Rust 风格现代，Python 类型提示完整 |
| **多语言协作** | ⭐⭐⭐⭐ Python+Rust+TS 混合，边界清晰 |

**最重要的设计思想**：

1. **Trait 抽象**：所有关键组件（ApiClient、ToolExecutor、PermissionPolicy）都是 trait，便于测试和替换
2. **不可变数据类**：Python 端所有模型用 `frozen=True`，确保数据一致性
3. **Hook 系统**：统一的 Pre/Post 钩子，插件可干预工具执行流程
4. **上下文压缩**：主动管理上下文长度，支持会话恢复

---
*报告生成时间：2026-04-02*
*分析范围：claw-code/src/*, rust/crates/runtime/*, rust/crates/plugins/*