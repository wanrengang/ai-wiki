---
title: Metis（自进化Agent双记忆框架）
created: 2026-06-25
updated: 2026-06-25
type: entity
tags: [agent, memory, self-evolution, architecture, training]
sources: [raw/papers/2026-06-25-metis-self-evolving-agents.md]
confidence: high
---

# Metis（自进化Agent双记忆框架）

## 概述

Metis 是首个在**相同经验集**上系统隔离研究文本记忆 vs 代码记忆的自我进化 Agent 框架。由港中文+华为+武汉大学团队提出（arXiv 2606.24151）。

核心发现：两种记忆形式在**构建成本、执行效率、可迁移性**三个维度上呈现互补权衡——**任何单一表示都不够用**。

## 关键事实

| 维度 | 文本记忆 | 代码记忆 |
|------|----------|----------|
| 构建成本 | 低（直接记录） | 高（需生成验证） |
| 执行效率 | 中（需推理复用） | 高（直接调用） |
| 可迁移性 | 好（跨任务适用） | 差（需重写适配） |

## 系统架构

Metis 构建**层次双表示记忆**：
- **文本记忆**：组织为执行计划、环境事实、常见陷阱，按需注入上下文
- **代码记忆**：将反复出现的计划结晶为可验证的工具，延迟生成以摊薄成本
- **记忆管理器**：基于复用频率决定何时将文本记忆升级为代码记忆
- **Reflection Harness**：引导文本→代码的记忆升级时机

## 核心数据

- **AppWorld 基准**：任务精度比 ReAct **+20.6%**
- **执行成本**：比 ReAct **−22.8%**
- 精度/成本综合表现优于代表性自进化 Agent 系统

## 与其他框架对比

Metis 的双记忆视角与以下框架互补：
- [[mem0]]：通用记忆层，但未区分文本/代码表示权衡
- [[decoction-experience-memory]]：记忆压缩，专注上下文效率
- [[mlevolve-self-evolving-mle-agent]]：MLE-Bench 驱动的自演化，侧重 benchmark 迭代
- [[self-compacting-agents]]：自适应轨迹压缩，专注 Agent 思维链压缩
- [[agent-native-memory-systems]]：评估框架，揭示无单一架构主导所有场景

## 关联

- [[test-time-compute-scaling]]：两条路线都在摊销推理成本
- [[agent-drift-multi-agent-systems]]：长期运行行为退化问题，Metis 通过记忆结晶缓解
