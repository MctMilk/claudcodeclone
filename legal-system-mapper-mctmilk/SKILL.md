---
name: legal-system-mapper-mctmilk
description: 以某个法条为核心节点，构建其上下游法条关联网络，包括上位规范、并列条款、下位细化、程序衔接、竞合分析。当用户想了解某个法条在法律体系中的位置时使用。
---

# 法条体系定位 (Legal System Mapper)

## 技能简介

七步法工作流**第三步**：找到法条的上下游关联，确保没有用错法条。

## 核心提示词

```
请以【法条编号】为核心节点，构建其法条关联网络：

1. 上位规范：该条的上位原则条款是哪条？
2. 并列条款：与该条处于同一层级、解决类似问题但适用条件不同的条款有哪些？请对比列表说明差异
3. 下位细化：有哪些司法解释条文、指导性案例对该条进行了细化？
4. 程序衔接：主张该条权利时，举证责任如何分配？诉讼时效如何计算？
5. 竞合分析：该条与哪些条文可能产生请求权竞合？当事人如何选择？各自利弊？
6. 请用思维导图的文字结构呈现以上关系
```

## 输出要求

- **法条关联图**：文字树形结构展示上下游关系
- **并列条款对比表**：对比差异点，便于选择适用
- **竞合分析建议**：告诉用户不同选择的风险收益

## 后续步骤

完成此技能后，建议继续：

- **证据映射**：`legal-evidence-mapping-mctmilk` - 基于体系定位选择最优法条后，对应证据
- **攻防模拟**：`legal-debate-simulation-mctmilk` - 确认用本条而非其他条，测试攻防
- **完整工作流**：`legal-seven-step-workflow-mctmilk` - 串联全部七步

---

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-structural-analysis-mctmilk` | 法条结构拆解（第一步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk) |
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖（第二步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-evidence-mapping-mctmilk` | 证据清单映射（第四步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk) |
| `legal-debate-simulation-mctmilk` | 攻防模拟陪练（第五步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk) |
| `legal-case-validator-mctmilk` | 类案验证（第六步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk) |
| `legal-decision-tree-mctmilk` | 决策流程图生成（第七步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
