---
name: legal-decision-tree-mctmilk
description: 基于前六步分析结果，为法条生成完整的适用决策流程图（SOP），标注判断节点、证据要求和切换条件。这是法条分析的最终沉淀，形成可复用的实战操作指南。
---

# 法条适用决策树生成 (Legal Decision Tree)

## 技能简介

七步法工作流**第七步**（最终沉淀）：把前六步的大量分析，固化成一张可复用的决策流程图。

## 前置条件

此技能应在完成以下任一步骤后使用：
- `legal-structural-analysis-mctmilk` - 结构化拆解
- `legal-concept-deep-dive-mctmilk` - 概念深挖
- `legal-system-mapper-mctmilk` - 体系定位
- `legal-evidence-mapping-mctmilk` - 证据映射
- `legal-debate-simulation-mctmilk` - 攻防模拟
- `legal-case-validator-mctmilk` - 类案验证

**建议至少完成前四步后再使用此技能。**

## 核心提示词

```
基于以上分析，请为【法条编号】生成一张完整的适用决策流程图：

要求：
1. 以"是否满足要件"为判断节点
2. 每个节点标注：需要审查的事实 + 关键证据 + 举证责任
3. 标出常见"岔路口"：在哪些节点最容易出现争议？争议如何处理？
4. 标出与其他法条的切换点：不满足本条时，可以转向哪条？
5. 最终到达：支持/驳回的结论 + 法律效果

格式（文字树形结构）：
START → 审查要件1（XXX）
  ├── 是 → 审查要件2（XXX）
  │         ├── 是 → ……（继续直到结论）
  │         └── 否 → [替代方案/驳回理由]
  └── 否 → [替代法条/驳回]
```

## 输出要求

- **决策流程图**：文字树形结构（ASCII格式）
- **判断节点标注**：事实要求 + 证据要求 + 举证责任
- **岔路口标注**：易争议点 + 处理方式
- **切换点标注**：不满足本条时的备选法条

## 示例输出格式

```
START
└── 审查：行为人是否实施了侵权行为？
    ├── 是 → 审查：是否造成损害后果？
    │         ├── 是 → 审查：行为与损害是否存在因果关系？
    │         │         ├── 是 → 审查：行为人是否有过错？
    │         │         │         ├── 是 → 结论：支持原告诉请
    │         │         │         └── 否 → 结论：按公平责任处理
    │         │         └── 否 → 结论：不构成侵权，驳回
    │         └── 否 → 结论：不构成侵权，驳回
    └── 否 → 结论：不构成侵权，驳回

【切换点】如不满足本条，可转向：
- 第1166条（无过错责任）
- 第1167条（公平责任）
```

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-structural-analysis-mctmilk` | 法条结构拆解（第一步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk) |
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖（第二步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-system-mapper-mctmilk` | 法条体系定位（第三步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk) |
| `legal-evidence-mapping-mctmilk` | 证据清单映射（第四步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk) |
| `legal-debate-simulation-mctmilk` | 攻防模拟陪练（第五步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk) |
| `legal-case-validator-mctmilk` | 类案验证（第六步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
