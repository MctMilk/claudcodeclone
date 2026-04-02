---
name: legal-debate-simulation-mctmilk
description: AI扮演对方律师，对己方的法条适用进行全方位攻击，并给出防御策略建议。这是庭前沙盘推演的核心技能。当用户想"自己打自己"、测试己方论点弱点时使用。
---

# 攻防模拟陪练 (Legal Debate Simulation)

## 技能简介

七步法工作流**第五步**（沙盘推演）：AI扮演对方律师攻击己方案法，发现弱点，补全防御。

## 核心提示词

```
我代理原告/被告，拟依据【法条编号】主张【具体诉请】。
案件基本事实如下：（简述关键事实）

请你扮演对方律师，对我的法条适用进行全方位攻击：

1. 要件攻击：我方哪个构成要件的事实基础最薄弱？如何攻击？
2. 法条选择攻击：对方是否会主张应适用其他法条而非本条？理由？
3. 解释攻击：该法条中哪个概念存在对我方不利的解释空间？
4. 时效/程序攻击：程序层面有无可攻击的点？
5. 价值衡量攻击：对方是否可从公平、诚信等原则层面挑战我的主张？

最后请给出：我方应对上述攻击的防御策略建议
```

## 使用方式

使用时需提供：
1. **己方立场**（原告/被告）
2. **法条编号和具体诉请**
3. **案件关键事实**（简述，不超过500字）

## 输出要求

- **攻击清单**：5个维度的具体攻击点
- **防御策略**：针对每个攻击点的应对方案

## 进阶用法

第一轮结束后，可以要求：
- "请再扮演我方律师，反攻对方"
- "把攻守角色互换，再来一轮"

## 后续步骤

完成此技能后，建议继续：

- **类案验证**：`legal-case-validator-mctmilk` - 用真实案例检验攻防策略
- **决策固化**：`legal-decision-tree-mctmilk` - 把攻防经验固化为决策流程图
- **完整工作流**：`legal-seven-step-workflow-mctmilk` - 串联全部七步

---

## 关联技能

| 技能 | 功能 | GitHub |
|------|------|--------|
| `legal-structural-analysis-mctmilk` | 法条结构拆解（第一步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-structural-analysis-mctmilk) |
| `legal-concept-deep-dive-mctmilk` | 不确定法律概念深挖（第二步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-concept-deep-dive-mctmilk) |
| `legal-system-mapper-mctmilk` | 法条体系定位（第三步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-system-mapper-mctmilk) |
| `legal-evidence-mapping-mctmilk` | 证据清单映射（第四步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-evidence-mapping-mctmilk) |
| `legal-case-validator-mctmilk` | 类案验证（第六步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-case-validator-mctmilk) |
| `legal-decision-tree-mctmilk` | 决策流程图生成（第七步） | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-decision-tree-mctmilk) |
| `legal-seven-step-workflow-mctmilk` | **七步法完整工作流（主技能）** | [链接](https://github.com/MctMilk/SkillsLab/tree/main/legal-seven-step-workflow-mctmilk) |

## 创意来源

基于杰足先登@AI智法引擎文章《7步AI法条拆解法》改编
https://mp.weixin.qq.com/s/1RPKLjU2_5ZFb7jGxXri3g
