---
title: Tmax — 终端 Agent 的简单 RL 配方
created: 2026-06-24
updated: 2026-06-24
type: entity
tags: [model, agent, training, benchmark, open-source]
sources: [raw/papers/2026-06-24-tmax-terminal-agents.md]
confidence: high
---

## 概述

Tmax 是艾伦人工智能研究所（Allen Institute for AI）和华盛顿大学提出的开源 RL 训练配方，专用于终端（Terminal）Agent。核心贡献：仅用 9B 参数即在 Terminal-Bench 2.0 达到 27%，超越参数量大得多的闭源模型（如 GLM-5、Claude-Sonnet-4.6）。

## 核心方法

**数据生成三重奏（Taxonomy）：**
- **难度控制（Difficulty Control）：** 按任务复杂度分层采样
- **人格（Personas）：** 多样化智能体人格提升泛化
- **验证器多样化（Verifier Diversification）：** 避免验证器过拟合

**训练配方：**
- 仅用**结果奖励（Outcome-only reward）** 的简化 RL 流程
- 不需要过程奖励或显式过程监督
- 完全开源（SFT 基座 + RL 配方）

## 关键结果

| 模型 | Terminal-Bench 2.0 |
|------|-------------------|
| Tmax (9B, open RL) | **27%** |
| GLM-5 (闭源) | ~25% |
| Claude-Sonnet-4.6 (闭源) | ~22% |
| Prior open models | <20% |

## 与现有工作的关系

Tmax 是 [[harness-engineering]] 中 "Harness 递归" 模式的技术基础 — 通过数据工程和 RL 配方提升 Agent 能力，而非扩大模型规模。

[[autolab-long-horizon-agent-benchmark]] 测试的也是长时序 Agent 任务，与 Tmax 关注终端多步操作互补。

[[agent-drift-multi-agent-systems]] 揭示了多 Agent 长期运行的行为退化问题；Tmax 的数据多样性设计（personas）正是针对分布漂移的一种缓解手段。

[[test-time-compute-scaling]] 的 Budget Forcing 等推理时方法与 Tmax 的 RL 训练方法形成对比：前者改变推理过程，后者改变模型本身。
