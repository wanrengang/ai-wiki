---
title: ToolBench-X
created: 2026-06-26
updated: 2026-06-26
type: concept
tags: [agent, benchmark, tool-use, reliability, evaluation]
sources: [raw/papers/2026-06-26-toolbench-x-benchmark-2606.25819.md]
confidence: high
---

# ToolBench-X

## 概述

**ToolBench-X**：上海交大提出的工具调用 Agent 评测基准，聚焦**工具环境不可靠性（Tool-Environment Unreliability）**。揭示了一个关键问题：在真实环境中，API 文档会过时、字段会被重命名、服务会超时、返回结果会不完整或语义偏移——但现有基准仍假设干净的、稳定的环境。

核心发现：**在同一组工具上表现良好的 agent，在可恢复的可靠性hazard下往往失败**。

## 核心设计

### 五类可靠性 Hazard

| Hazard 类型 | 描述 | 示例 |
|------------|------|------|
| **Specification Drift** | 工具规范漂移 | API 文档过时/字段被包装 |
| **Invocation Error** | 调用错误 | 参数歧义/缺失必需参数 |
| **Execution Failure** | 执行失败 | 服务超时/服务不可用 |
| **Output Drift** | 输出漂移 | schema 或 surface form 变化 |
| **Cross-source Conflict** | 跨源冲突 | 不同工具提供矛盾证据 |

### 关键设计原则

**所有注入的故障必须是可恢复的** — 每个故障都保留了至少一条有效恢复路径（重试、回退、交叉验证、归一化或证据验证）。

### 与现有基准对比

| 基准 | 任务数 | 工具数 | Spec Drift | Inv Error | Exec Fail | Output Drift | Cross-source | Sequential | Parallel | Mixed |
|------|--------|--------|------------|-----------|-----------|--------------|--------------|------------|----------|-------|
| **ToolBench-X** | 1106 | 4956 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| BFCL v3 | 1000 | N/R | × | 部分 | × | × | × | ✓ | ✓ | ✓ |
| ToolBench | 126,486 | 16,464 | × | × | × | × | × | ✓ | × | × |
| AnyToolBench | 400 | >16,000 | × | × | × | × | × | ✓ | × | × |

## 核心发现

### 1. 可靠性鸿沟（Reliability Gap）

在干净工具环境下表现良好的 agent，在可恢复 hazard 下普遍失败。这是核心发现：当前 agent 的工具调用能力≠真实环境任务完成能力。

### 2. 失败根源：hazard 诊断能力不足

失败更多来自：
- **有限的 hazard 诊断能力**（不能识别工具输出有问题）
- **无效的恢复策略**（知道有问题但不知道怎么恢复）

而非：
- 工具调用数量不足
- 推理预算不足（test-time scaling 效果有限）

### 3. 定向恢复提示的作用

提供定向的恢复提示（而非更多推理步骤）可以恢复大量失败任务。

### 4. 超越函数调用

评估应该从"模型能否调用正确的工具？"进化到"工具环境不确定时，模型能否完成任务？"

## 核心结论

> **工具调用准确率 ≠ 任务完成能力**。评估工具使用能力必须超越函数调用的正确性，进入在不可靠工具环境下的鲁棒任务完成。

## 相关链接

- [[tool-use-rl-collapse]] — RL 训练中的结构崩溃（互补：本文评估工具环境，本文研究 RL 算法）
- [[harness-engineering]] — Harness 工程（关注工程约束与 agent 性能的关系）
- [[hypertool]] — 超越逐步工具调用范式
- [[skillweaver-compositional-skill-routing]] — 工具组合路由问题
- [[agent-production-evaluation]] — Agent 生产评估（benchmark 能筛选能力起点但不等同生产验收）
