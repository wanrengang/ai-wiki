---
title: AtomMem
created: 2026-06-20
updated: 2026-06-20
type: concept
tags: [memory, AI-Agent, long-term-memory, retrieval, architecture]
sources: [raw/papers/2026-06-20-atommem-atomnic-fact-memory-llm-agents.md]
confidence: high
---

# AtomMem

## Overview

AtomMem（中科大）是一个以**原子事实（Atomic Facts）为中心**的 LLM Agent 长期记忆系统，核心目标是解决现有记忆系统的两个根本矛盾：

1. **存储密度 vs 上下文保真度**：存储原始对话信息量大但噪音多；压缩表示紧凑但丢失细节并积累幻觉噪声
2. **记忆稳定性 vs 动态演化**：LLM 驱动的无约束更新引入不稳定性，错误编辑反复修改同一记忆条目导致记忆腐化

## Architecture

AtomMem 由四个协同组件构成：

### 3.1 Fact Executor（原子事实提取器）
- **SFT 微调的 Qwen3-14B**（LoRA rank=128, A100 单卡训练 3 epochs）
- 从嘈杂的原始对话中选择高价值原子事实，执行共指消解和时间锚定
- 生成**自包含的原子事实**：无需外部上下文即可独立理解
- **Structured Atomic Fact** 结构：`F = {id, content, embedding, P(participants), K(keywords), T(temporal), E(event_ids)}`

### 3.2 Fact Verification（事实验证）
- 新事实入库前与已有记录做**混合相似度**比对（语义 embedding + keyword Jaccard）
- LLM 分析新输入与检索上下文的关Xiv系：生成**残差内容**（存储为新事实）或**更新元组**（检测到逻辑冲突时）
- 防止记忆冗余，维持全局一致性

### 3.3 Event Memory（事件记忆）
- 将相关事实聚合成**连贯的叙事块**（Episode）
- `Event E = {id, summary, fact_ids, participants, keywords, temporal_span}`
- 动态吸收新对齐的事实入现有事件，或创建新事件

### 3.4 Temporal Profile（时序画像）
- 从积累的事实证据中建立**用户状态时序图谱**
- 增量追踪稳定用户属性，同时适应偏好漂移并保留历史信息
- 支持时间约束查询，选择特定时间段内的画像版本

### 3.5 Associative Memory Graph（联想记忆图）
- 通过**实体重叠、共享事件、对话连续性**连接事实
- 使用 **Random Walk with Restart (RWR)** 实现联想召回
- 将孤立事实转化为连接的知识网络

### 3.6 Hierarchical Retrieval（三层检索）
```
用户查询 → 意图分析（LLM提取 P_q, K_q, T_q, I_prof）
         → Primary Recall（参与者+时间过滤 → 混合相似度排序 → top-k/2）
         → Compensatory Recall（事件层扩展 → 融合得分 → top-k/2）
         → Associative Recall（图传播RWR → top-k_f）
         → Profile Augmentation（如需则追加用户画像）
         → LLM 生成最终响应
```

## Performance

**LoCoMo Benchmark（GPT-4o-mini backbone）：**

| Method | F1 | BLEU-1 | J (LLM-as-Judge) | Tokens(K) |
|--------|-----|--------|---------------------|-----------|
| LoCoMo | 37.81 | 26.23 | 41.67 | 827 |
| MemoryBank | 17.85 | 12.13 | 23.96 | 927 |
| A-Mem | 33.87 | 27.64 | 32.29 | 11,688 |
| **MEM0** | 54.95 | 44.71 | 54.17 | 55,300 |
| MemoryOS | 49.72 | 43.58 | 51.04 | 19,208 |
| LightMem | 49.30 | 42.45 | 43.75 | 5,022 |
| **AtomMem-Flat** | 47.08 | 40.40 | 52.08 | **723** |
| **AtomMem** | **56.66** | **49.56** | **64.58** | 21,357 |

关键洞察：
- **AtomMem 全链路 SOTA**，超越 MEM0、MemoryOS 等
- **AtomMem-Flat**（去除层级/事件/画像/图结构）仅需 723K tokens 即达到竞争性能，MEM0 需要 55,300K（77倍差距）
- 记忆密度经济性：AtomMem 的原子事实表示使 token 消耗降低 2.6 倍（vs MemoryOS）同时性能更高

## 与同类系统的关系

AtomMem 的原子事实提取与 [[mem0]] 的记忆层级有相似思路，但通过 SFT 提取器和验证机制解决了 mem0 的无约束更新问题。与 [[mnemis-ai-long-memory]] 相比，两者都采用图结构组织记忆，但 Mnemis 的 Kahneman 双系统检索 vs AtomMem 的三层 RWR 检索走了不同路线。[[decoction-experience-memory]] 的"中等记忆规模最优"洞察在 AtomMem 的 Fact Verification 中有类似体现——通过去重和冲突检测控制记忆膨胀。

## 核心洞察

1. **原子事实作为最小语义单元**：将嘈杂对话转化为自包含事实，比原始对话更紧凑，比压缩摘要更忠实
2. **可验证的增量更新**：Fact Verification 机制取代无约束 LLM 重写，解决记忆腐化问题
3. **图传播的联想召回**：RWR 在记忆图上传播激活，连接跨会话的碎片化记忆
4. **记忆密度经济学**：AtomMem-Flat 仅 723K tokens 达到 MEM0 55,300K 的竞争性能

## Links

- [[mem0]] — 对比：mem0 无约束更新 vs AtomMem 验证机制
- [[mnemis-ai-long-memory]] — 对比：Kahneman 双系统 vs 三层 RWR 检索
- [[decoction-experience-memory]] — 关联：中等记忆规模最优原则
- [[agent-memory-systems-characterization]] — 关联：记忆系统评估框架
- [[rag]] — 关联：记忆增强 RAG 范式
- [[test-time-compute-scaling]] — 关联：推理时计算分配
- [[harness-engineering]] — 关联：Agent 工程基础设施
- [[projectmem]] — 关联：代码 Agent 记忆系统
