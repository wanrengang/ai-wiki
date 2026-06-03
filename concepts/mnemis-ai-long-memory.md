---
title: Mnemis
created: 2026-05-28
updated: 2026-05-28
type: concept
tags: [mnemis, long-term-memory, AI-memory, graph-index, dual-process, ACL2026]
sources: [raw/articles/2026-05-28-mnemis-ai-long-memory-acl2026.md]
confidence: high
---

# Mnemis

微软提出的 AI 长期记忆框架，ACL 2026 主会议接收。对应 [[test-time-compute-scaling]] 中的 System-1/System-2 双过程理论，将「建构主义」认识论引入记忆系统设计。

## 核心问题：传统 RAG 的「近视眼」困境

传统 RAG 依赖语义相似度检索，存在三个根本局限：
1. **孤立评分** — 每条记忆独立与查询比较，忽略记忆间关系
2. **语义偏见** — 向量相似度偏爱字面匹配，对间接相关信息天然不敏感
3. **无法推理** — 不了解对话历史中存在哪些话题及相互关系

典型案例：问"Dave 2023 年去过哪些城市"，RAG 找到 Boston 和 San Francisco 但遗漏 Detroit（"attended a conference in Detroit"埋藏在长消息中）。

## 核心设计：建构式索引 + 双系统检索

### 索引阶段：自适应层级图

受认识论中「建构主义」启发，将碎片化对话组织为**两层图结构**：

| 层级 | 名称 | 作用 |
|------|------|------|
| L1 | Base Graph（知识图谱） | 实体提取、消歧、去重、聚合，消除碎片化 |
| L2 | Hierarchical Graph（层级图） | 将具体实体归纳为高层语义概念，建立跨主题高阶连接 |

层级图三原则：最小概念抽象（MCA）、多对多映射（M2M）、压缩效率约束（CEC）。

### 检索阶段：双系统融合

受 Kahneman 双系统理论启发，融合两条检索路径：

- **System-1（快思考）**：查询向量化，在 Base Graph 中快速匹配语义最相似实体
- **System-2（慢思考）**：利用 LLM 推理能力，在层级图上自顶向下逐层遍历；可触发 Shortcut 机制直接获取某类别下所有后代节点

两条路径**融合互补**：System-1 确保语义直接匹配不遗漏，System-2 确保结构相关但语义距离较远的内容被覆盖。

## 关键洞察

> 记忆系统的质量，很大程度上取决于「存储时做了什么」，而不仅仅是「检索时怎么找」。

传统 RAG 将所有智能放在检索阶段，索引阶段几乎是无加工的分块向量化。Mnemis 在索引阶段就进行深度语义建构，使检索阶段能同时利用快速匹配和结构遍历——对应人类记忆的两个关键特征：**存储时的建构性**和**提取时的双模式**。

## 实验结果

| 基准 | 准确率 | 说明 |
|------|--------|------|
| LoCoMo | 93.9% | GPT-4.1-mini 底座 |
| LongMemEval-S | 91.6% | GPT-4.1-mini 底座 |

显著优于现有 RAG 和 Graph-RAG 方法。

## 相关概念

- [[rag]] — 传统 RAG 是 Mnemis 试图克服的基础范式
- [[mem0]] — AI Agent 通用记忆层，与 Mnemis 解决同一问题域的不同路径
- [[decoction-experience-memory]] — 中等记忆规模最优结论，与长记忆研究互补
- [[test-time-compute-scaling]] — System-1/System-2 双过程理论是 Mnemis 检索阶段的核心启发
- [[ragflow]] — 深度文档理解 RAG 引擎，文档级检索的另一种思路

## 来源

- 论文：https://arxiv.org/abs/2602.15313
- GitHub：https://github.com/microsoft/Mnemis