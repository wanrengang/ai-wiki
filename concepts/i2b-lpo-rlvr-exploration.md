---
title: I²B-LPO — RLVR 探索突破（ACL 2026）
created: 2026-05-17
updated: 2026-05-17
type: concept
tags: [optimization, training, reasoning, agent, inference]
sources: [raw/articles/2026-05-17-i2b-lpo-acl2026-rlvr.md]
confidence: high
---

# I²B-LPO — RLVR 探索突破（ACL 2026）

## Overview

I²B-LPO（Implicit-Information Bottleneck Latent Policy Optimization）来自阿里达摩院智能决策团队，被 ACL 2026 Main 接收。核心解决 RLVR（Reinforcement Learning with Verifiable Rewards）中 rollout 采样「思维同质化」问题——模型收敛到少数高概率模板，生成轨迹表面不同但逻辑同质。

## Core Problem

**RLVR 的探索-利用困境：**
- 盲目增加采样次数不能带来更高效的探索，因为轨迹共享相似的底层逻辑，削弱了 reward 差异和 advantage 信号
- GRPO 训练中，精度早早 plateau，但 response length 和 4-gram 重复率持续上升——模型在生成更长、更重复的内容，而非更有效的推理

**两种现有熵方法的缺陷：**
| 问题 | 描述 |
|------|------|
| Reward Hacking | 模型生成无意义发散（冗长、重复、偏题）来操控熵奖励 |
| Inductive Bias | token 级别扰动只能改变表面表达，无法打破预训练推理偏好 |

## Two Key Mechanisms

### 1. High-Entropy Branching（高熵节点分支）

**核心思路：** 在高熵位置（真正的关键决策点）进行 latent variable branching，而不是均匀采样。

```
Hₜ = -Σ p(token) × log p(token)
分支条件: Hₜ > τ (高百分位阈值)
```

**PSA（Pseudo Self-Attention）注入：**
- 用 latent variable 调制 RMSNorm scaling
- 将 latent variable 映射为额外的 K/V，拼接到原始 attention
- PSA 作为「隐式思维提示」，持续影响后续推理轨迹

### 2. Information Bottleneck Self-Reward（信息瓶颈自奖励）

**质量评分：**
```
Score = I(r; a) - β × I(q; r)

I(r; a) = 轨迹对最终答案的信息贡献
I(q; r) = 防止冗余的约束项
```

**过滤策略：** 保留 IB score 最高的 Top-N 轨迹用于 GRPO 策略更新。

低 IB score 轨迹的三种典型问题：
1. **Empty Verbosity**：过多的"Let me think"填充
2. **Repetitive Loops**：重述问题但不产生新推理
3. **Logical Drift**：表达正确但公式或推导方向错误

## Workflow

```
Initial Rollout → High-Entropy Branching → Generate Candidate Trajectories → IB Self-Reward Filtering → GRPO Policy Update
```

## Experimental Results

| 数据集 | 指标 | 提升 |
|--------|------|------|
| MATH | Accuracy | Up to +5.3% |
| MATH | Semantic Diversity | Up to +7.4% |

训练数据：6,486 MATH + 13,583 DAPO 样本

## Relationship to Existing Work

- [[dypo-dynamic-policy-optimization]] — DYPO 也是 SFT/RL 协同优化，但 DYPO 按样本难度动态路由，I²B-LPO 按推理节点熵值分支，思路互补
- [[on-policy-distillation-thunlp]] — OPD 关注思维模式一致性，I²B-LPO 关注 RLVR 的探索效率问题
- [[test-time-compute-scaling]] — 同样关注推理效率，TTCS 侧重推理时计算分配，I²B-LPO 侧重训练时轨迹多样性
- [[tool-hallucination-reasoning-trap]] — 推理增强方法的可靠性问题，I²B-LPO 通过高熵分支改善推理质量

## Resources

- Paper: https://arxiv.org/pdf/2601.05870
- Code: https://github.com/denghuilin-cyber/IIB-LPO
