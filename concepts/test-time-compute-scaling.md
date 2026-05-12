---
title: Test-Time Compute Scaling 测试时计算扩展
created: 2026-05-12
updated: 2026-05-12
type: concept
tags: [reasoning, inference, optimization, training, model]
sources: [raw/articles/lilian-weng-why-we-think-2025.md]
confidence: high
---

# Test-Time Compute Scaling 测试时计算扩展

## 概述

Test-time compute（测试时计算）指在推理阶段分配额外计算资源来提升模型输出质量。与预训练时 scaling laws 不同，test-time scaling 研究的是：如何在已知模型参数的情况下，通过分配更多推理算力来提升效果。

核心问题：不同难度的问题需要不同量的"思考时间"。简单问题直接回答最优，复杂推理问题则需要搜索和自我修正。

## 核心框架：CoT 作为隐变量

Chain-of-thought 可以建模为隐变量 $z$：

$$P(y \mid x) = \sum_{z \sim p(z \mid x)} P(y \mid x, z)$$

这解释了为什么并行采样多条推理路径或搜索推理空间能提升效果——本质是在隐空间中进行积分。^[raw/articles/lilian-weng-why-we-think-2025.md]

## 思维链进化史

| 工作 | 贡献 |
|------|------|
| Ling et al. 2017 | 引入数学中间步骤（AQUA-RAT 数据集） |
| Cobbe et al. 2021 | GSM 数据集；generator + verifier 架构 |
| Nye et al. 2021 | "Scratchpads"：中间思考 token |
| Wei et al. 2022 | 提出"Chain-of-Thought"提示词概念 |
| Kojima et al. 2022 | Zero-shot CoT："think step by step" |

关键发现：更大的模型从思考时间中获益更多。^[raw/articles/lilian-weng-why-we-think-2025.md]

## 并行采样策略

### Best-of-N
最简单策略：采样 N 次，选择得分最高的。

### Beam Search
自适应搜索，在有前景的路径上投入更多算力。

### Process Reward Model (PRM)
在每步推理上评估奖励，引导 beam search 候选选择：

> "Per-step self-evaluation reduces accumulative errors in multi-step reasoning during beam search decoding." — Xie et al. 2023

### Self-Consistency (Wang et al. 2023)
多条 CoT 轨迹的多数投票。^[raw/articles/lilian-weng-why-we-think-2025.md]

## 自纠正：纯 RL 能学会"反思"吗？

### 自纠正的三大失败模式
LLM 的纯自纠正**无法正常工作**，失败模式包括：
1. **幻觉**：修改了正确的回复
2. **行为崩溃**：几乎不修改首个回复
3. **分布偏移泛化失败**

自纠正必须依赖外部反馈：ground truth 匹配、单元测试结果、更强模型的反馈或人类反馈。^[raw/articles/lilian-weng-why-we-think-2025.md]

### SCoRe: 通过 RL 实现自纠正

两阶段训练：
- **Stage 1**：最大化第二次尝试准确率 + 第一次尝试的 KL 惩罚
- **Stage 2**：同时优化两次尝试的准确率

Stage 1 防止行为崩溃（模型对首个回复只做微小编辑）。^[raw/articles/lilian-weng-why-we-think-2025.md]

## DeepSeek-R1：纯 RL 催生"Aha Moment"

### 训练流程
```
Cold-start SFT → Reasoning RL → Rejection Sampling + SFT → Final RL
```

| 阶段 | 描述 |
|------|------|
| Cold-start SFT | 在数千条冷启动数据上微调基座模型（提升可读性） |
| Reasoning RL | 基于规则的奖励：格式（`<thinking>` 标签）+ 准确率 |
| Rejection Sampling + SFT | 80 万样本，2 个 epoch；推理+非推理任务混合 |
| Final RL | 提升有用性、无害性、推理能力 |

### 关键发现
- **纯 RL（无 SFT）可以学会反思和回溯**（"Aha moment"）
- 模型自然学会花费更多思考 token
- 即使是 64K token 的超长 CoT 也能从正确的奖励信号中自然涌现

### 失败尝试
- **PRM**：难以定义每步评判标准，容易被 reward hacking
- **MCTS**：语言搜索空间过大，训练细粒度 value model 困难 ^[raw/articles/lilian-weng-why-we-think-2025.md]

## 测试时计算何时有效？

| 情况 | 最优策略 |
|------|----------|
| 简单问题 | 直接回答（无需 CoT） |
| 中等难度 | 标准 CoT |
| 困难推理 | 搜索 + PRM / Self-Consistency |
| 极难（需搜索） | Beam search + 多数投票 |

**结论**：测试时计算 scaling 与训练时计算 scaling 的规律不同，需要自适应策略。^[raw/articles/lilian-weng-why-we-think-2025.md]

## Budget Forcing

通过强制控制思考长度来调节推理：
- **延长**：追加 "wait" 延长思考
- **缩短**：追加 end-of-thinking token 或 "Final Answer:"

即使模型没有自然延伸 CoT，budget forcing 也能提升性能。^[raw/articles/lilian-weng-why-we-think-2025.md]

## CoT 忠实性问题

Lanham et al. 2023 发现三种失败模式：
1. **提前作答**：模型在 CoT 生成前已形成结论
2. **无信息 Token**：填充词（如句号）不提升性能
3. **人类不可读编码**：改写不降低准确率 = CoT 是事后合理化

o3 和 o4-mini 展示了改进的 CoT 忠实性——它们真正用思考 token 处理信息，而非生成后的事后解释。^[raw/articles/lilian-weng-why-we-think-2025.md]

## 与隐式推理的对比

[[laser-implicit-reasoning]]（ACL 2026）代表了另一条路线：用概率叠加在 token 内部实现隐式推理，Token 消耗降低 97%+，无需生成显式思维链。

两种路线对比：
| 维度 | 显式 CoT（Test-time Compute） | 隐式推理（Laser） |
|------|------------------------------|------------------|
| Token 消耗 | 高（需要生成思考链） | 极低 |
| 可解释性 | 高（显式推理步骤） | 低 |
| 适用场景 | 复杂推理任务 | 资源敏感场景 |
| 代表工作 | DeepSeek-R1, SCoRe | Laser, DWAL |

## 相关概念

- [[laser-implicit-reasoning]] — 隐式视觉推理（Token 消耗降低 97%+）
- [[anthropic-claude-roadmap-2026]] — Claude 下一代方向（含多智能体协作）
- [[dario-amodei-2028-prediction]] — Dario Amodei 2028 预测（60% 概率 AI 自我制造）
- [[lenvm]] — LenVM：Token 级长度控制

## 参考

- Wei et al. 2022 - "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"
- Wang et al. 2023 - "Self-Consistency Improves Chain of Thought Reasoning"
- Lanham et al. 2023 - "Measuring Faithfulness in Chain-of-Thought Reasoning"
- DeepSeek Team - DeepSeek-R1 训练流程
- Lilian Weng - "Why We Think" (2025-05-01)
