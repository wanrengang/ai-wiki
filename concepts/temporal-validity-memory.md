---
title: Temporal Validity in Retrieval Memory (MemStrata)
created: 2026-06-27
updated: 2026-06-27
type: concept
tags: [agent, memory, rag, architecture, inference]
sources: [raw/papers/2026-06-27-memstrata-temporal-validity-retrieval-memory.md]
confidence: high
---

## 概述

MemStrata（Temporal Validity in Retrieval Memory）是由 MemStrata.dev / Called It Inc. 提出的**确定性过时事实消除层**，专门解决 AI Agent 在动态知识环境中检索到过期信息的问题。核心洞察：RAG 没有时间模型，这是结构性缺陷而非调优问题。

## 核心问题：RAG 的时间盲区

当事实发生变化时（函数重命名、配置版本更新、API 重构），RAG 会以几乎相同的 embedding 相似度同时检索到过期值和当前值，无法判断哪个更新。

论文关键实验数据：
- **余弦相似度区分矛盾事实 vs 重复改写**：AUROC = 0.59（接近随机）
- **矛盾事实平均比改写重复更具 embedding 相似性**（违反直觉）
- 这不是调参能解决的——是结构性问题

结论：Agent 在需要回答时，RAG 有 **15-40% 的概率返回过期值**。

## 解决方案：双时态账本 + 确定性supersession规则

MemStrata 在保留 RAG 全部静态知识召回能力的同时，引入：

1. **Bi-temporal ledger（双时态账本）**：每条事实记录注册时间和失效时间
2. **确定性 supersession 规则**：`subject, relation, object` 三元组被新断言矛盾时，过期值被确定性退役
3. **无需相似度阈值、无需 LLM 调用**

```
关键机制：
- 新事实进来 → 检查 subject+relation 是否已存在
- 若存在且被新值矛盾 → 确定性标记为 superseded
- 无需 embedding 比较，纯规则驱动
```

## 实验结果（7B 本地模型，消费级硬件）

| Benchmark 类型 | RAG | MemStrata |
|---|---|---|
| 静态知识（project-fact QA）| 基线 | 基本持平（无召回损失）|
| 静态知识（multi-session dialogue）| 基线 | 基本持平 |
| 动态（code mutation）| 0.20-0.47 | 0.95-1.00 |
| 动态（configuration migration）| 0.20-0.47 | 0.95-1.00 |
| 动态（dependency bumps）| 0.20-0.47 | 0.95-1.00 |
| 动态（API evolution）| 0.20-0.47 | 0.95-1.00 |

**过时事实错误率（核心指标）**：RAG 15-40% → MemStrata ≈ 0%

## 与现有 Agent Memory 系统的关系

- [[agent-native-memory-systems]] 评估了 12 个系统，无一专门处理时态有效性
- [[atommem-atomnic-fact-memory]] 用 SFT 提取器解决记忆腐化，但方法不同（原子事实提取 vs 确定性 supersession）
- [[rag]] 完全没有时间模型——MemStrata 揭示了 RAG 作为 Agent 记忆层的根本局限
- [[decoction-experience-memory]] 关注经验提精的规模效应，不处理知识时效性

## 核心启示

> "这是检索增强生成无法通过构造匹配的确定性层"——不是调优问题，是架构问题。

对于在代码库、API 文档、配置文件中操作的 AI Agent，过时事实是高频、高impact的故障模式。MemStrata 的贡献在于形式化了这个问题并给出了无需 LLM 的确定性解。

## 相关概念

- [[rag]] — RAG 没有时间模型，MemStrata 在 RAG 基础上添加时间有效性层
- [[atommem-atomnic-fact-memory]] — 另一种记忆更新机制（原子事实 + SFT 验证）
- [[decoction-experience-memory]] — 经验提精，中等记忆规模最优
- [[mem0]] — 通用的 Agent 记忆层，无时间有效性机制
- [[agent-native-memory-systems]] — 12 个主流系统的系统级评估
