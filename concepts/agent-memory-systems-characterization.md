---
title: Agent Memory Systems 表征研究
created: 2026-06-07
updated: 2026-06-07
type: concept
tags: [agent, inference, memory, benchmark, research]
sources: [raw/papers/2026-06-07-agent-memory-characterization-2606.06448.md]
confidence: high
---

# Agent Memory Systems 表征研究

## 概述

**论文：** Agent Memory: Characterization and System Implications of Stateful Long-Horizon Workloads  
**arXiv:** [2606.06448](https://arxiv.org/abs/2606.06448) | 04 Jun 2026  
**作者：** Yasmine Omri (Stanford), Ziyu Gan, et al.

首个对 Agent 记忆系统进行**系统级表征**的研究。覆盖 10 个代表性系统，揭示设计选择如何非对称地转移成本（写入路径 vs 读取路径）。

## 核心贡献

1. **四轴分类法**：按 construction、storage、retrieval、mutability 四个维度对记忆系统分类
2. **相位感知 Profile Harness**：追踪 tokens、model calls、GPU 利用率、延迟（construction / retrieval / generation）
3. **10 系统表征**：覆盖 construction cost shape、capability thresholds、amortization structure、freshness scheduling、footprint growth、retrieval tail behavior
4. **10 条系统建议**：涵盖 construction scheduling、capability floors、amortization via query volume、freshness-latency tradeoffs、fleet-scale management

## Agent Memory 执行流水线

7 个阶段：ingestion → memory construction → storage → retrieval → prompt assembly → generation → maintenance

关键发现：不同设计选择将成本**非对称地**重新分配——将历史压缩为原子事实的系统大幅削减查询时 prompt 长度，但付出的 construction 时 LLM prefill 成本高出数量级。

## 四类记忆范式

| Paradigm | System | Construction | Storage | Retrieval | Mutability |
|----------|--------|-------------|---------|----------|------------|
| I: Long-context | long_context | ✗ | ✗ | passthrough | append |
| II: Flat RAG | BM25 | ✗ | inverted index | lexical top-k | append |
| II: Flat RAG | EmbedRAG | ✗ | dense store | dense top-k | append |
| III.a: Struct-RAG (append) | GraphRAG | LLM | graph store | graph expand | append |
| III.a: Struct-RAG (append) | HippoRAG v2 | LLM | multi-view graph | PPR rerank | append |
| III.b: Struct-RAG (consolidate) | [[mem0]] | LLM | dense store | fact top-k | consolidate |
| III.b: Struct-RAG (consolidate) | SimpleMem | LLM | hybrid store | iterative hybrid | consolidate |
| IV: Agentic control | A-Mem | LLM | graph store | graph map-reduce | mutate |
| IV: Agentic control | Letta | LLM | blocks+archive | tool calls | mutate |
| IV: Agentic control | MIRIX | LLM | multi-store DB | routed typed | mutate |

## 关键发现

### 长上下文的三大根本局限
1. **容量限制**：多会话交互和工具 traces 不可避免地超出任何固定上下文预算 → 外部记忆是功能性先决条件
2. **Prefill 成本**：随历史长度二次扩展；前缀缓存在跨会话时失效（缓存驱逐 + KV-cache 内存压力）
3. **召回退化**：模型在长序列中表现出"U 形"性能曲线——上下文中间的事实经常丢失，有效上下文长度通常是声称窗口的一小部分

### 与现有研究的关系
- 与 [[mnemis-ai-long-memory]] (ACL 2026) 的层级图索引 + Kahneman 双系统检索角度互补：MNEMIS 关注检索机制，本文关注系统级成本建模
- 与 [[mem0]] 的对比：Mem0 是 III.b consolidating 型代表，本文将其与其他 9 系统横向表征
- 与 [[maple-sub-agent-architecture]] 的对比：MAPLE 聚焦架构分解，本文聚焦记忆子系统的系统行为

## 10 条系统建议（论文）

1. Construction scheduling optimization
2. Capability floors for memory systems
3. Amortization via query volume
4. Freshness-latency tradeoffs
5. Fleet-scale management
6-10. （待补充，详见原文 Section 5）

## 相关概念

- [[mem0]] — 记忆系统对比表中的代表性系统
- [[mnemis-ai-long-memory]] — ACL 2026 记忆框架，层级图索引
- [[maple-sub-agent-architecture]] — ALA 2026，记忆三元分解
- [[harness-engineering]] — Agent 工程基础设施，与本文 profile harness 概念相关
- [[rag]] — Agent Memory 将 RAG 从静态检索泛化到可变状态管理