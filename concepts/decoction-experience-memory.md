---
title: Decoction of Experience（经验提精）：AI Agent 记忆优化新范式
created: 2026-05-15
updated: 2026-05-15
type: concept
tags: [agent, memory, inference, training, reasoning]
sources: [raw/articles/2026-05-15-decoction-experience-llm-agents-2604.04373.md]
confidence: high
---

# Decoction of Experience（经验提精）：AI Agent 记忆优化新范式

## 概述

**Decoction（煎煮/提精）** = 从经验中提取本质，有组织地整理，并检索突出信息以构建有效上下文。论文（arXiv:2604.04373）核心论点：**原始经验本身是不够的——经验必须被提精才能成为有用的上下文。**

这与 [[test-time-compute-scaling]] 形成互补：test-time scaling 研究如何通过增加推理时的计算量来提升效果；decoction 则研究如何通过**改进输入上下文的质量**来诱导更高效的推理。

## 核心发现

### 1. 经验提精优于原始经验

从原始经验（每个问题 m=4 条轨迹）中：
- **成功尝试** → 总结关键洞察和推理模式
- **失败尝试** → 反思常见错误和推理缺陷

经验提精后的效果优于直接使用原始经验，在 agentic 任务中表现更好，并带来更好的输入可扩展性。

### 2. 记忆整合的「甜蜜点」

- 性能在**中等记忆规模时达到最优**——中等记忆可以**超越完整记忆**的表现
- 记忆整合（Memory Consolidation）是与经验提精并列的第二关键因素
- 这意味着 [[rag]] 不是塞得越多越好，而是需要策略性「遗忘」

### 3. 相关性-多样性平衡

有效上下文必须同时满足：
- **Relevance（相关性）**：直接适用于查询的信息
- **Diversity（多样性）**：覆盖不同问题类型和解决策略

### 4. 概念树结构（Concept Tree）

将提取的概念组织成**层级树结构**，跨概念组进行组织：
- 改进上下文构建
- 鼓励从更多样化但仍相关的概念组中检索
- 优于基线平面记忆方法

## 与 RAG 的区别

| Approach | Focus | Decoction's Distinction |
|----------|-------|------------------------|
| RAG | 外部知识检索 | Agent 上下文需要**策略**，不仅仅是证据 |
| Decoction | 经验内部提精 | 从成功/失败经验中主动提取教训 |

> 「与其增加输出预算，不如向 Agent 提供精心构建的输入（即上下文）来诱导更高效的推理。」

## 系统架构

### 训练阶段
```
Problems {xᵢ} → Trajectories + Rewards {oᵢ} → Raw Memory → Memory Organization → Organized Memory M̃
```

### 测试阶段
```
New Query x → Retrieval R(x; M̃) → Context Constructor → LLM πθ → Output ŷ
```

## 实验验证

| 任务 | 训练数据 | 评估数据 | 关键指标 |
|------|----------|----------|----------|
| 数学推理 | DAPO-Math (14,116) | AMC/AIME/HMMT (260) | Exact-match |
| WebShop | 3,930 trajectories | 200 episodes | Purchase quality |
| SWE-bench | 1,794 instances | SWE-bench Verified (500) | Patch passes |

关键超参数：K=8（数学检索数）、L=2（概念树层级）、α=0.5（混合权重）

## 对 Agent 工程的影响

[[ai-agent]] 的记忆设计不应只是「塞更多经验」，而是需要有组织地提精和结构化。[[mem0]] 作为通用记忆层，其实也面临类似挑战——记忆不是越多越好，需要有策略地整合和检索。

Decoction 的「甜蜜点」发现对工程实践有重要启示：与其追求完整记忆，不如追求**高质量、有组织、可检索的精选记忆**。
