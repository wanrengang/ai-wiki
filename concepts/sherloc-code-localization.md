---
title: SHERLOC（代码修复诊断定位框架）
created: 2026-06-25
updated: 2026-06-25
type: concept
tags: [agent, coding, fault-localization, inference]
sources: [raw/papers/2026-06-25-sherloc-code-repair-agents.md]
confidence: high
---

# SHERLOC（代码修复诊断定位框架）

## 概述

**SHERLOC (Structured Hypothesis-driven Exploration and Reasoning for Localization)** 是 NVIDIA + TU Darmstadt 提出的代码修复 Agent 诊断定位框架（arXiv 2606.24820）。

核心问题：LLM Agent 解决仓库级编码任务时，**50%预算花在定位故障而非修复上**。现有定位框架只做文件检索，不提供修复所需的诊断上下文。

SHERLOC 是**无需训练、无需微调、无需多 Agent 编排**的框架，通过结构化诊断提升定位质量。

## 核心成果

| 基准 | 结果 |
|------|------|
| SWE-Bench Lite accuracy@1 | **84.33%** |
| SWE-Bench Verified recall@1 | **81.27%** |
| ~30B 参数规模 | 匹配或超越其他 Agent 方法 |

将 SHERLOC 定位结果注入修复 Agent，下游任务平均显著提升。

## 关键设计原则

- **推理原生工具使用**：不是简单文件检索，而是提供修复所需诊断上下文
- **紧凑仓库工具套件**：避免大海捞针式搜索
- **自我纠错机制**：在迭代交互循环中自我修正

## 与现有工作的关系

- [[agentic-code-reasoning]]：CoT 半形式化推理框架，分析代码行为；SHERLOC 专注定位诊断，两者互补
- [[coda-arxiv-2605.19269]]：Claude Code 生成光速 GEMM 内核；SHERLOC 解决修复前的定位问题
- [[harness-engineering]]：关注运行时稳定性和纠错回路；SHERLOC 是 Harness 在代码定位环节的具体实现

## 核心洞察

**定位≠文件检索，定位=可操作诊断**。现有 benchmark 评估定位为文件检索，但修复 Agent 需要：
1. 故障位置
2. 故障类型（哪类错误）
3. 诊断上下文（为什么在这里出错）

三者齐备，才能真正推进修复。
