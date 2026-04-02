---
name: legal-seven-step-workflow-mctmilk
description: 完整的人工智能辅助法条拆解七步工作流，每一步都受四条铁律约束，确保分析质量。律师处理任何法条案件时，从头到尾跑完七步，生成完整的法条分析报告。
---

# 4条铁律约束的七步法法律工作流

## 创意来源

本技能基于微信公众号"AI智法引擎"作者杰足先登的文章《7步AI法条拆解法：年轻律师用完直呼"早该知道"》改编。

原文链接：https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g

---

## 四条铁律（贯穿全程）

> ⚠️ **铁律一：AI是副驾驶，不是主驾驶**
> 所有AI的分析，都必须经过你自己的专业判断复核。AI不会出庭，不会承担责任，你才会。

> ⚠️ **铁律二：案例必须验证**
> AI可能生成看起来真实的案号和裁判要旨，但它们可能根本不存在。逢案必验，去中国裁判文书网、北大法宝交叉验证。

> ⚠️ **铁律三：时效性警觉**
> AI训练数据有截止日期。新修订的法条、新出的司法解释，AI未必知道。新法用旧框，后果自负。

> ⚠️ **铁律四：个案差异**
> AI给的是一般性框架，你面对的是具体案件。事实细节的精细化适配，永远是人工完成的。

---

## 七步完整工作流

### 开始之前

准备好：
- 要分析的法条原文（粘贴全文）
- 涉及的案件类型或场景（简述）

---

### 🔵 第一步：结构化拆解 → `legal-structural-analysis-mctmilk`

**功能**：对法条进行七维结构拆解（规范类型/适用主体/构成要件/法律效果/但书例外/关键概念）

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk`

**输出物**：七维度分析表

---

### 🔵 第二步：概念深挖 → `legal-concept-deep-dive-mctmilk`

**功能**：深入剖析法条中的不确定法律概念（立法原意/学理通说/司法解释/裁判标准/边界案例/常见误区）

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk`

**输出物**：判断要素清单、边界案例

---

### 🔵 第三步：体系定位 → `legal-system-mapper-mctmilk`

**功能**：以法条为核心节点，构建上下游关联网络（上位规范/并列条款/竞合分析/举证责任/诉讼时效）

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk`

**输出物**：法条关联网络图

---

### 🔵 第四步：证据映射 → `legal-evidence-mapping-mctmilk`

**功能**：把法条要件翻译成证据清单，生成要件→事实→举证方→证据→反驳点→补强的完整对应表

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk`

**输出物**：六列证据作战地图

---

### 🔵 第五步：攻防模拟 → `legal-debate-simulation-mctmilk`

**功能**：AI扮演对方律师，对己方法条适用进行五维度攻击，并给出防御策略

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk`

**输出物**：攻击清单 + 防御策略

---

### 🔵 第六步：类案验证 → `legal-case-validator-mctmilk`

**功能**：用真实案例验证分析，揭示裁判分歧、高频败诉原因、法官审查重点（**必须核验AI案例**）

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk`

**输出物**：裁判分歧报告

---

### 🔵 第七步：决策固化 → `legal-decision-tree-mctmilk`

**功能**：把前六步分析固化为可复用的决策流程图（SOP），判断节点/证据要求/切换点

**GitHub**：`https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk`

**输出物**：法条专属决策流程图

---

## 最终交付物清单

完成七步后，你应该手握：

- [ ] 七维度结构拆解表
- [ ] 关键概念深挖报告
- [ ] 法条关联网络图
- [ ] 证据作战地图（六列表）
- [ ] 攻防模拟报告（含防御策略）
- [ ] 类案验证报告（已核验）
- [ ] 法条适用决策流程图（SOP）

---

## 使用示例

**用户说**：
> "帮我用七步法分析《民法典》第1165条"

**AI回答**：
> 我们现在开始七步法完整工作流。

**第一步**，请提供法条原文，或者直接告诉我你想分析哪个法条？

---

## 注意事项

- 这是一个**串联型**技能，每次处理一个法条，从第一步做到第七步
- 中途如果用户喊停，记录当前做到哪一步，下次可以继续
- 如果案件特别复杂，可以对单个步骤重复使用（比如多个模糊概念就多次第二步）
- **铁律二是底线**，第六步的核验绝对不能跳过
