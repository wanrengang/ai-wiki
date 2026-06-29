---
title: Self-Compacting — Agent 轨迹自适应压缩
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [agent, inference, optimization, memory]
sources: [raw/papers/2026-06-24-self-compacting-agents.md]
confidence: high
---

## 定义

Self-Compacting（SELF-COMPACT）是一种终端 Agent 推理时压缩框架，允许模型自主决定何时以及如何对积累的思维链和工具调用历史进行压缩，避免上下文窗口溢出。

## 核心问题

长 Agent 轨迹（多步思维链 + 工具调用）会积累大量无用内容，导致：
1. **上下文窗口耗尽：** 轨迹超过 LLM 的上下文限制
2. **陈旧内容锚定：** 已失效的中间结果影响后续生成
3. **现有方案缺陷：** 固定 token 阈值触发压缩，不关心轨迹结构，可能在推导中途或搜索过程中丢弃有用的部分结果

## 解决方案：两个推理时元素

**① 压缩工具（Compaction Tool）：**
模型主动调用一个专门的摘要工具来压缩积累的上下文。

**② 触发规则（Rubric）：**
轻量级规则指定何时触发/抑制压缩：
- **触发（Fire）：** 子任务已解决，或轨迹趋于收敛时
- **抑制（Suppress）：** 推导进行中，或模型陷入困境时

两者缺一不可：单独使用压缩工具会导致模型在不恰当的时刻调用；单独使用规则则无法实际执行压缩。

## 无需微调

SELF-COMPACT 完全在推理时工作，不需要任何微调或外部监督，适用于任何支持工具调用的开源模型。

## 实验结果

在 6 个基准（竞争数学 + Agent 搜索）和 7 个模型上验证：
- 匹配或超越固定间隔摘要（Fixed-Interval Summarization）
- 自适应触发比固定阈值更高效地利用上下文预算

## 相关概念

[[decoction-experience-memory]] 同样研究 Agent 记忆压缩策略 — Decoction 关注经验知识的提炼，SELF-COMPACT 关注轨迹结构压缩，两者技术路径不同但目标相似。

[[mem0]] 是通用的 AI Agent 记忆层框架，与 SELF-COMPACT 的端侧压缩机制在设计上有共通之处。

[[agent-memory-systems-characterization]] 系统性评测了 10 个记忆系统，SELF-COMPACT 的自适应触发机制是其中一个重要维度。

[[test-time-compute-scaling]] 的 Budget Forcing 通过强制延长/缩短思维链来分配推理时算力；SELF-COMPACT 则通过压缩历史来扩展有效上下文窗口 — 两者都是推理时资源分配策略。
