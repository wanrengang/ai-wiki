---
title: Agent-Native Memory Systems 评估
created: 2026-06-25
updated: 2026-06-25
type: concept
tags: [agent, memory, benchmark, architecture]
sources: [raw/papers/2026-06-25-agent-native-memory-2606.24775.md]
confidence: high
---

# Agent-Native Memory Systems 评估

## 概述

来自上交（SJTU）和清华的系统性研究，对 12 个代表性 Agent Memory 系统进行数据管理视角的评估。论文指出：现有评估仍以端到端任务成功率（F1、BLEU）为主，将底层系统视为黑盒，忽视了运维成本、架构权衡和动态知识更新下的鲁棒性。

## 四模块分析框架

论文将 Agent Memory 分解为四个核心模块：

| 模块 | 职责 | 关键挑战 |
|------|------|----------|
| Memory Representation & Storage | 记忆的表示与持久化 | 表示保真度 |
| Extraction | 从交互中提取信息写入记忆 | 更新正确性 |
| Retrieval & Routing | 查询和路由记忆片段 | 检索精度 |
| Maintenance | 记忆的 consolidation 和生命周期管理 | 长时稳定性 |

## 评估结论

- **无单一架构主导所有场景** — 效果高度依赖于记忆结构与工作负载瓶颈的对齐程度
- **局部维护（localized maintenance）比全局重组（global reorganization）更具成本效益**
- 细粒度消融实验量化了各模块对表示保真度、检索精度、更新正确性和长时稳定性的影响
- 代码已公开：`https://github.com/OpenDataBox/MemoryData`

## 被评估的 12 个系统

涵盖：Mem0、LLM Powered Agents 中的记忆方案、以及学术研究系统。评估在 5 个 benchmark 工作负载、11 个数据集上进行。

## 关键发现

1. **表示保真度** vs **检索精度** 之间存在权衡
2. **动态知识更新**场景下，许多系统的鲁棒性不足
3. **运维成本**（Token 消耗、延迟）在真实工作负载下被首次系统量化
4. 未来方向：构建真正 Agent-Native 的记忆系统——即记忆结构由 Agent 执行模式驱动，而非反过来

## 相关概念

[[wikilinks]]
- [[mem0]] — AI Agent 通用记忆层（48K Stars），被本论文评估
- [[decoction-experience-memory]] — 经验提精，中等记忆规模最优
- [[agent-memory-systems-characterization]] — Stanford 首个 10 系统级 Agent 记忆表征研究
- [[atommem-atomnic-fact-memory]] — AtomMem 原子事实记忆，SFT 提取器解决记忆腐化
- [[test-time-compute-scaling]] — 推理时计算Scaling，记忆是 Scaling 的重要维度
- [[agent-drift-multi-agent-systems]] — 多智能体系统长期行为退化，记忆退化是核心因素之一

## 参考

- 论文：Are We Ready For An Agent-Native Memory System? (arXiv 2606.24775)
- 作者：Wei Zhou, Xuanhe Zhou (SJTU), Guoliang Li (Tsinghua), et al.
- GitHub: https://github.com/OpenDataBox/MemoryData
