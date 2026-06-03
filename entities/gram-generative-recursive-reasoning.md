---
title: GRAM - Generative Recursive Reasoning Models
created: 2026-05-24
updated: 2026-05-24
type: entity
tags: [model, architecture, reasoning, inference]
sources: [raw/articles/2026-05-24-bengio-gram-recursive-reasoning.md]
confidence: high
---

# GRAM - Generative Recursive Reasoning Models

图灵奖得主 Yoshua Bengio 提出的生成式递归推理模型，将确定性递归潜在推理转变为概率性多轨迹计算。

## 核心创新

**问题：** 现有递归推理模型（RRMs）是确定性的——给定相同输入，只能沿一条固定轨迹前进，无法探索多个有效解（如 N-Queens 问题），且易陷入局部最优。

**方案：** 在递归推理每一步引入可学习的随机性：
- 确定性模块计算「提议更新」u_t
- 从状态相关高斯分布采样「随机引导信号」ε_t
- z_t = h_t + l_t，其中 h_t = μ_θ(x, τ_{<t}) + ε_t, ε_t ~ N(0, σ²_θ(x, τ_{<t}))

**层次化架构：**
- 低层组件 l：每次转移内部更新 K 次，精细计算
- 高层组件 h：每次转移更新一次，抽象推理状态
- 随机性仅注入高层，不干扰低层精细计算

## 关键结果

- **16 步递归 + 20 条并行采样 > 320 步串行递归**：超越所有确定性基线
- **并行轨迹探索**：突破单轨迹限制，能同时发现多个有效解
- **双轴扩展：** 深度（串行）+ 宽度（并行）灵活权衡

## 候选选择机制

1. **多数投票（Majority Voting）**：选择出现频率最高的预测
2. **LPRM（潜在过程奖励模型）**：训练值函数 v_ψ(z_t) 直接预测轨迹质量

## 与 LeCun 观点的呼应

LeCun 最近播客中明确表态：LLM 自回归生成不是通往 AGI 的正确路径，真正的智能应在潜在空间中通过规划和推理实现。GRAM 正是这一方向的实证探索。

## 训练方法

变分推断，最大化 ELBO：
- 重构项：鼓励从采样轨迹得到正确预测
- KL 散度项：约束后验与先验距离

训练时后验能看到正确答案，推理时只用先验采样。

## 相关概念

- [[test-time-compute-scaling]] — 推理时计算扩展，与 GRAM 的双轴扩展策略密切相关
- [[agent-drift-multi-agent-systems]] — 多智能体长期运行退化，GRAM 的多轨迹探索可能是缓解路径之一
- [[lilian-weng-why-we-think]] — Lilian Weng 对推理时计算的深度解析，包含 Budget Forcing、Self-Correction 等方法

## 来源

- 论文：https://arxiv.org/abs/2605.19376
- 项目主页：https://ahn-ml.github.io/gram-website