---
name: legal-evidence-mapping-mctmilk
description: 将法条构成要件对应当事人需要证明的事实、举证责任分配和可用证据类型，生成完整的证据作战地图。这是律师实务中最核心的技能。当用户想"把法条翻译成证据清单"时使用。
---

# 要件事实证据对应 (Legal Evidence Mapping)

## 技能简介

七步法工作流**第四步**（核心实务）：把法条翻译成证据清单，生成完整的要件-事实-证据三层对应表。

## 核心提示词

```
我正在处理一个涉及【法条编号】的案件。

请帮我完成"要件—事实—证据"的三层对应分析：

针对该法条的每一个构成要件：
1. 【要件名称】
   - 需要证明的具体事实是什么？
   - 举证责任由哪方承担？（依据是什么？）
   - 可以提交哪些类型的证据来证明？（列出至少3种）
   - 对方可能的反驳方向是什么？
   - 如果该要件证据薄弱，有什么补强策略？

请逐一分析每个要件，最后用表格汇总：
| 要件 | 待证事实 | 举证方 | 优选证据 | 对方反驳点 | 补强方案 |
```

## 输出要求

- **要件-事实-证据三层对应表**：六列完整表格
- **举证责任说明**：明确哪方举证及法律依据
- **补强策略**：针对证据薄弱环节的具体建议

## 后续步骤

完成此技能后，建议继续：

- **攻防模拟**：`legal-debate-simulation-mctmilk` - 基于证据清单进行攻防演练
- **类案验证**：`legal-case-validator-mctmilk` - 验证证据要求是否符合司法实践
- **决策固化**：`legal-decision-tree-mctmilk` - 把证据要求固化为流程图
- **完整工作流**：`legal-seven-step-workflow-mctmilk` - 串联全部七步

---

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-structural-analysis-mctmilk` | 法条结构拆解（第一步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk) |
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖（第二步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-system-mapper-mctmilk` | 法条体系定位（第三步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk) |
| `legal-debate-simulation-mctmilk` | 攻防模拟陪练（第五步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk) |
| `legal-case-validator-mctmilk` | 类案验证（第六步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk) |
| `legal-decision-tree-mctmilk` | 决策流程图生成（第七步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
