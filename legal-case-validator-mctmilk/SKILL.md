---
name: legal-case-validator-mctmilk
description: 用真实案例验证法条分析结果，分析司法实践中的裁判分歧、高频败诉原因和法官审查重点。AI生成的分析必须用真实案例验证。当用户想检验法条分析是否符合司法实践时使用。
---

# 类案检索与验证 (Legal Case Validator)

## 技能简介

七步法工作流**第六步**（验证关）：用真实案例检验前五步分析是否符合司法实践。**铁律二强制执行：必须核验AI案例。**

## 核心提示词

```
请针对【法条编号】，帮我分析该条在司法实践中的适用情况：

1. 典型适用场景：该条最常见的3类案件类型是什么？
2. 裁判分歧：实务中对该条的适用是否存在裁判分歧？主要分歧点是什么？
3. 指导性案例：是否有最高法指导性案例涉及该条？裁判要旨是什么？
4. 高频败诉原因：当事人援引该条但败诉的最常见原因是什么？（至少3个）
5. 法官审查重点：法官在审查该条适用时，最关注哪几个点？
6. 实务操作建议：基于以上分析，在实务中运用该条应注意的要点清单
```

## ⚠️ 铁律二强制执行

**AI生成的案例信息，务必去以下数据库交叉验证：**
- 中国裁判文书网
- 北大法宝
- 法信

AI可能生成看起来真实的案号和裁判要旨，但**它们可能根本不存在**。逢案必验，是底线。

## 输出要求

- **典型案件类型**：3类最常见场景
- **裁判分歧点**：说明分歧原因和各自裁判观点
- **高频败诉原因**：至少3个，要具体
- **法官审查重点**：给用户实务操作建议

## 后续步骤

完成此技能后，建议继续：

- **决策固化**：`legal-decision-tree-mctmilk` - 把验证结论固化为决策流程图
- **完整工作流**：`legal-seven-step-workflow-mctmilk` - 串联全部七步

---

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-structural-analysis-mctmilk` | 法条结构拆解（第一步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk) |
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖（第二步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-system-mapper-mctmilk` | 法条体系定位（第三步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk) |
| `legal-evidence-mapping-mctmilk` | 证据清单映射（第四步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk) |
| `legal-debate-simulation-mctmilk` | 攻防模拟陪练（第五步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk) |
| `legal-decision-tree-mctmilk` | 决策流程图生成（第七步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
