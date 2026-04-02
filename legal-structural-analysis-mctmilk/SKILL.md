---
name: legal-structural-analysis-mctmilk
description: 对法条进行结构化拆解，分析规范类型、适用主体、构成要件、法律效果等七个维度。当律师或法律学习者需要"拆解"法条、理解法条骨架时使用。
---

# 法条结构化拆解 (Legal Structural Analysis)

## 技能简介

七步法工作流**第一步**：拿到法条后先"拆骨"，看清骨架。

## 核心提示词

```
请对以下法条进行结构化拆解：

【法条全文】：（粘贴法条原文）

请按以下维度逐一分析：
1. 规范类型：授权性、义务性还是禁止性规范？
2. 适用主体：谁是义务主体？谁是权利主体？
3. 构成要件：逐一列出触发该法条适用的全部要件
4. 法律效果：满足要件后产生什么法律后果？
5. 但书/例外：是否存在除外情形？
6. 关键法律概念：标出需要进一步解释的不确定法律概念
7. 请用一张表格汇总以上内容
```

## 输出要求

- **表格汇总**：用 Markdown 表格呈现七维度分析结果
- **构成要件清单**：每个要件单独列出，标注清楚
- **不确定概念标注**：标出需要深挖的模糊词

## 示例

输入：`《民法典》第1165条 过错责任原则`

输出：规范类型/适用主体/四个构成要件（行为/损害/因果关系/过错）/关键概念标注（`过错`、`因果关系`）

## 后续步骤

完成此技能后，建议继续：

- **概念深挖**：`legal-concept-deep-dive-mctmilk` - 深挖标注的不确定法律概念
- **体系定位**：`legal-system-mapper-mctmilk` - 了解法条上下游关联
- **完整工作流**：`legal-seven-step-workflow-mctmilk` - 串联全部七步

---

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-system-mapper-mctmilk` | 法条体系定位 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk) |
| `legal-evidence-mapping-mctmilk` | 证据清单映射 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk) |
| `legal-debate-simulation-mctmilk` | 攻防模拟陪练 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-debate-simulation-mctmilk) |
| `legal-case-validator-mctmilk` | 类案验证 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk) |
| `legal-decision-tree-mctmilk` | 决策流程图生成 | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
