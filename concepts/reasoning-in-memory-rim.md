---
title: Reasoning in Memory (RiM)
created: 2026-05-30
updated: 2026-05-30
type: concept
tags: [reasoning, latent-reasoning, test-time-compute, optimization, inference]
sources: [raw/papers/2026-05-30-rim-reasoning-in-memory-llm.md]
confidence: high
---

# Reasoning in Memory (RiM)

## 概述

**Reasoning in Memory (RiM)** 是由 JKU Linz 和 NXAI 提出的一种潜在推理方法，核心思想是用**固定记忆块（Memory Blocks）**替代自回归生成的推理步骤，实现单次前向传播内的并行推理。论文发表于 ACL 2026 Main Conference。

## 核心问题

现有 CoT 推理将中间计算耦合到自回归生成，存在两个根本问题：
1. **效率瓶颈**：每个推理步骤必须完全生成后才能被后续计算使用
2. **算力浪费**：语言是通信优化的载体，生成中间推理步骤浪费了大量算力在语法正确的文本生成上

## 方法：记忆块

**记忆块 = 固定序列的特殊 token**，不是生成的，而是预先定义的。

### 两阶段训练课程

**Stage 1（建立工作记忆容量）**：
- 在每个记忆块后监督预测下一个推理步骤
- 强迫 LLM 将任务相关的中间信息编码到记忆块中
- 类比人类"出声思考"阶段

**Stage 2（去掉中间监督）**：
- 去掉推理步骤监督，改为在每个记忆块后预测最终答案
- 训练信号从"预测推理步骤"转向"逐步精炼答案"
- 类比人类"内部化思考"

### 注意力掩码设计

```
记忆块 → 关注问题 + 之前的记忆块（黄色）
目标推理步骤 → 关注之前记忆块 + 问题（不关注其他推理步骤）
→ 强制推理压缩到记忆块内部
```

**关键优势**：所有 readout 可在单次前向传播中并行训练，无信息泄漏。

## 实验结果

- **训练**：GPT-2 / Llama-3.2 规模，GSM8K-Aug 数据集
- **评估**：GSM8K（in-distribution）+ GSM-Hard（out-of-distribution）
- **结果**：匹配或超越现有 CoT 和潜在推理基线，同时实现推理并行化

## 与 Test-Time Compute Scaling 的关系

RiM 是 [[test-time-compute-scaling]] 的另一条路线：
- **CoT/Budget Forcing/RL 自纠正**：逐步生成推理步骤（串行）
- **RiM**：固定记忆块 → 单次前向传播（并行）
- **共同目标**：在不改变模型参数的情况下扩展推理算力

## 相关概念

- [[test-time-compute-scaling]] — 测试时算力扩展的完整图谱
- [[laser-implicit-reasoning]] — 隐式视觉推理框架（Token 消耗降低 97%+）
- [[agentic-code-reasoning]] — 半形式化代码推理
- [[lilian-weng-why-we-think]] — LLM 推理机制深度解析
- [[mnemis-ai-long-memory]] — 记忆框架（层级图索引）

## 参考

- **论文**：Unlocking the Working Memory of Large Language Models for Latent Reasoning (arXiv:2605.30343)
- **作者**：Lukas Aichberger, Sepp Hochreiter (ELLIS Unit Linz, NXAI GmbH)
- **认证**：ACL 2026 Main Conference