---
title: DYPO - 动态策略优化
created: 2026-05-18
updated: 2026-05-18
type: concept
tags: [optimization, training, reinforcement-learning, fine-tuning]
sources: [raw/articles/2026-05-18-dypo-dynamic-policy-optimization.md]
confidence: high
---

# DYPO - 动态策略优化

## 概述

DYPO（Dynamic Policy Optimization）是由清华大学深圳国际研究生院、中兴通讯、重庆邮电大学联合提出的 SFT+RL 协同训练方法。核心洞察是：**不同样本需要不同的优化路径**，而非将 SFT 和 RL loss 无差别混合。

**论文**：[arXiv 2604.08926](https://arxiv.org/pdf/2604.08926) | [GitHub](https://github.com/Tocci-Zhu/DYPO)

## 核心问题：偏差-方差矛盾

| 方法 | 偏差 | 方差 | 特点 |
|------|------|------|------|
| **SFT** | 高 | 低 | 学得快、收敛稳，但易过拟合示范分布，压缩探索空间 |
| **RL** | 低 | 高 | 更具探索性，但采样随机性和奖励稀疏性导致梯度方差高、训练易波动 |

## DYPO 核心机制

DYPO 根据 rollout 结果将样本划分为三类，匹配不同优化路径：

### 样本三分类

| 类型 | 判断标准 | 优化方法 | 目的 |
|------|----------|----------|------|
| **Easy 样本** | 全部 rollout 成功 | 直接跳过 | 减少无效更新 |
| **Hard 样本** | 全部 rollout 失败 | 多教师蒸馏 (Multi-Teacher Distillation) | 建立可靠先验 |
| **Mid 样本** | 有成功也有失败 | RL + GAL | 最优学习前沿 |

### 核心技术：Group Alignment Loss (GAL)

利用同一组 rollout 中的成败轨迹差异，显式将模型拉向正确轨迹、推离错误轨迹。相当于在纯 GRPO 基础上增加了一个低方差的相对对齐约束。

**理论保证：**
- 多教师蒸馏：组合多个 teacher 可抵消个体偏差，监督偏差随 teacher 数量增加而下降
- GAL：混合目标梯度方差严格小于纯 GRPO，且随模型区分轨迹能力提升，GAL 方差会进一步衰减

## 实验结果

### Qwen2.5-Math-7B

| 任务类型 | SFT→RL 基线 | DYPO | 提升 |
|----------|-------------|------|------|
| ID 平均 | 47.7 | **52.5** | +4.8 |
| OOD 平均 | 48.3 | **61.6** | +13.3 |

### Qwen3-4B-Base

| 任务类型 | SFT→RL 基线 | DYPO | 提升 |
|----------|-------------|------|------|
| ID 平均 | 56.1 | **66.9** | +10.8 |
| OOD 平均 | 52.6 | **68.5** | +15.9 |

## 训练动态

- **自适应课程**：训练初期依赖监督信号（teacher 扶着走），后期逐步转向策略自主探索
- **梯度稳定性**：DYPO 更新轨迹平滑，标准 GRPO 梯度曲线剧烈震荡

## 局限性与未来方向

1. **任务范围**：主要验证数学与逻辑推理场景，开放式对话、创作类任务有效性待验证
2. **算力开销**：每个 prompt 需要生成 8 条 rollout 以稳定估计样本难度

## 相关概念

- [[test-time-compute-scaling]] — DYPO 解决的是训练时优化问题，与 test-time scaling 互补
- [[maple-sub-agent-architecture]] — MAPLE 的个性化记忆系统可与 DYPO 的动态优化结合
- [[harness-engineering]] — DYPO 体现了"把正确信号给正确样本"的设计哲学

^[raw/articles/2026-05-18-dypo-dynamic-policy-optimization.md]
