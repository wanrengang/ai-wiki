---
title: DeepSeek-V4
created: 2026-06-19
updated: 2026-06-19
type: entity
tags: [model, architecture, inference, long-context, deepseek]
sources: [raw/papers/2026-06-19-deepseek-v4-million-token-context.md]
confidence: high
---

# DeepSeek-V4

## Overview

DeepSeek-V4 是 DeepSeek-AI 发布的百万 token 上下文 MoE 大模型预览版，包含两个变体：
- **DeepSeek-V4-Pro**：1.6T 总参 / 49B 激活参，支持 1M token 上下文
- **DeepSeek-V4-Flash**：284B 总参 / 13B 激活参，支持 1M token 上下文

核心突破：1M token 上下文场景下，V4-Pro 仅需 V3.2 的 **27% 单 token 推理 FLOPs** 和 **10% KV Cache**，Flash 更是低至 10% FLOPs / 7% KV Cache。

## Architecture

三大架构创新：

1. **混合注意力 CSA + HCA**（Compressed Sparse Attention + Heavily Compressed Attention）
   - CSA：将每 m 个 token 的 KV cache 压缩为 1 个条目，再做 DeepSeek Sparse Attention（DSA）
   - HCA：更激进的压缩，每 m' 个 token 压缩为 1 个条目但保持 dense attention
   - 两者交错配置，大幅降低长文本场景下的注意力计算成本

2. **Manifold-Constrained Hyper-Connections（mHC）**
   - 增强传统残差连接，将残差映射矩阵约束在双随机矩阵流形（Birkhoff 多面体）上
   - 保证映射矩阵谱范数 ≤ 1，残差变换非扩张，提升深层堆叠的数值稳定性
   - 输入/输出变换通过 Sigmoid 约束为非负有界

3. **Muon 优化器**
   - 替代传统 Adam，训练收敛更快、稳定性更高

其他继承自 V3 的设计：
- **DeepSeekMoE** FFN 架构（细粒度路由专家 + 共享专家）
- **Multi-Token Prediction（MTP）** 多 token 预测策略
- 激活函数从 Sigmoid 改为 Sqrt(Softplus())
- 初始若干 Transformer block 的 dense FFN 层替换为 Hash 路由 MoE 层

## Training

- DeepSeek-V4-Flash：32T tokens 预训练
- DeepSeek-V4-Pro：33T tokens 预训练
- Post-training 两阶段范式：
  1. 独立领域专家培育（SFT + GRPO 强化学习）
  2. 通过 on-policy distillation 将专家能力整合到统一模型（reverse KL loss）

## Evaluation Results

| 维度 | DeepSeek-V4-Pro-Max 表现 |
|------|--------------------------|
| 知识（SimpleQA） | 显著超越领先开源模型，接近 Gemini-3.1-Pro |
| 推理 | 超越 GPT-5.2 和 Gemini-3.0-Pro，落后 GPT-5.4/Gemini-3.1-Pro 约 3-6 个月 |
| Agent | 与 Kimi-K2.6、GLM-5.1 持平；超越 Claude Sonnet 4.5，接近 Opus 4.5 |
| 长上下文 | 1M token 学术基准超越 Gemini-3.1-Pro |

## Key Innovations Summary

- **效率**：1M token 场景下 FLOPs 降至 V3.2 的 27%，KV Cache 降至 10%
- **原生 1M 上下文**：无需任何外推技术，直接支持百万 token
- **FP4 路由专家权重**：MoE 路由专家参数使用 FP4 量化
- **两阶段 Post-training**：领域专家独立训练 → 统一模型蒸馏

## Related Entities

- [[ds4c]] — Redis 之父为 DeepSeek V4 Flash 打造的专用推理引擎
- [[ds4c-deepseek-v4]] — ds4c × DeepSeek V4 合作项目
- [[test-time-compute-scaling]] — Test-Time Compute Scaling 趋势下，长上下文是重要方向
- [[gated-deltanet-2]] — 另一篇同期 NVIDIA 高效注意力架构工作
- [[vortex-sparse-attention]] — 稀疏注意力 Serving 系统，AI Agent 自动生成算法
