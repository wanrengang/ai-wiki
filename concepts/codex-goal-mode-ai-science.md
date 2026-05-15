---
title: Codex Goal Mode 与 AI 科研奇点
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [agent, inference, research, coding]
sources: [raw/articles/2026-05-14-codex-goal-mode-scientific-breakthrough.md]
confidence: medium
---

# Codex Goal Mode 与 AI 科研奇点

## 概述

OpenAI Codex 的 `/goal` 模式在一次机械可解释性（Mechanistic Interpretability）实验中，AI 用 1 小时 56 分钟完成了人类博士约 80 小时的任务，效率提升约 **40 倍**。实验由 Agentic AI 工程师 Dan McAteer 发起，使用配置：**Codex /goal + GPT-5.5 High + Fast Mode**。

## Codex /goal 是什么

OpenAI Codex 工程师 Philip Corey 描述：`/goal` 是对 **Ralph Loop** 的一种实现——让目标在多轮对话里持续存在，不达成不停止。

- 普通 Codex：对话式，你说一句、它做一步
- Codex /goal：目标驱动，你说一个目标，它自己拆分子任务、执行、review、继续，直到达成或失败

这是从**对话式 AI**到**目标驱动 AI**的工程切换。

## 科研效率的拐点

- **SWE-bench**：2023 年底 Claude 2 通过率 2% → 2026 年 93.9%
- **GPQA Diamond**（博士级科学问答）：2023 年 GPT-4 得 39%（人类专家 65%）→ 2026 年 Gemini 3.1 Pro 得 94.3%，Claude Opus 4.7 得 94.2%
- **Darwin Gödel Machine**（Sakana AI）：代码自我改进，在 SWE-bench 上从 20.0% 提升到 50.0%，全程无人类介入

## RSI 概率

Anthropic 联创 Jack Clark（2026-05-07）：到 2028 年底 AI 实现完全递归自我改进（RSI）的概率 **>60%**。

## 相关概念

- [[test-time-compute-scaling]] — Codex /goal 是 Test-Time Scaling 的工程体现
- [[ai-scientists-autonomous-research]] — AI Scientist 项目（Nature 2026）已在 Nature 发表
- [[openai-frontier]] — Codex 是 OpenAI 的 Agent 产品
- [[hermes-agent]] — Hermes Agent 同样实现了 /goal 模式（11 天三厂同步上线）
