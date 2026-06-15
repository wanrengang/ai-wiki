---
title: Recursive Agent Harnesses (RAH)
created: 2026-06-15
updated: 2026-06-15
type: concept
tags: [agent, architecture, inference, multi-agent, training]
sources: [raw/papers/2026-06-15-recursive-agent-harnesses-2606.13643.md]
confidence: high
---

# Recursive Agent Harnesses (RAH)

## 概述

RAH 是 PwC 提出的递归 Agent Harness 模式（arXiv 2606.13643），将 RLM 的模型递归扩展为 Harness 递归——父 Agent 生成可执行脚本，并发生成子 Agent Harness，实现并行细粒度工作负载分解。

## 核心思想

两个独立工作线的汇合：
1. **RLM（Recursive Language Models）**：递归调用模型本身进行长上下文推理
2. **生产级 Coding Agent**：Anthropic dynamic workflows 等已在代码中生成子 Agent

RAH 将递归单元从"裸模型调用"升级为"完整 Agent Harness"（文件系统工具 + 代码执行 + 规划能力）。

## 架构

```
Parent Agent
  ├─ 代码执行生成模式：写入可执行脚本，并发生成子 Agent Harness
  └─ JSON 工具调用模式：适用于 1-5 个子任务

Subagent Harness（与父Agent同构，可递归分解）
  └─ 受限于可配置深度上限
```

## 关键设计

- **同构递归**：子 Agent 拥有与父 Agent 相同的生成能力，支持递归分解
- **并行 Spawn**：代码执行模式支持并行生成多个子 Harness
- **深度限制**：可配置递归深度上限，防止无限递归

## 实验结果

在 Oolong-Synthetic 基准（199 样本，13 个上下文长度桶，最长 4M tokens）：
- backbone = GPT-5（与 Codex/RLM 论文对齐）：71.75% → **81.36%**（提升 9.61pp）
- backbone = Claude Sonnet 4.5：达到 **89.77%**
- 提升完全来自 Harness 设计，而非模型本身

## 与 Related Concepts 的关系

- [[harness-engineering]] — RAH 是 Harness 递归化的具体实现，PE→CE→HE 演进的一环
- [[codex-goal-mode-ai-science]] — Codex Goal Mode 与 RAH 均探索 Agent 递归，但 RAH 扩展到完整 Harness
- [[openai-frontier]] — 企业级 Agent 平台，RAH 是其底层递归能力的理论支撑
- [[claude-managed-agents]] — Anthropic 托管平台，RAH 的并行 Harness 模式是其 dynamic workflows 的前身
- [[test-time-compute-scaling]] — RAH 通过递归 Harness 实现推理时计算扩展

## 延伸阅读

- 论文：arXiv 2606.13643
- 作者：Elias Lumer, Sahil Sen, Kevin Paul, Vamse Kumar Subbiah（PwC）