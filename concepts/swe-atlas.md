---
title: SWE Atlas
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [benchmark, agent, coding, engineering, evaluation]
sources: [raw/articles/2026-06-03-swe-atlas-scale-ai-coding-benchmark.md]
confidence: high
---

# SWE Atlas

## 概述

SWE Atlas 是 Scale AI 发布的 AI 编程智能体工程能力评估基准，聚焦代码理解、测试编写、代码重构三大被主流基准忽视的工作流，对前沿模型进行工程严谨度评估。论文链接：[arXiv 2605.08366](https://arxiv.org/pdf/2605.08366v1)。

## 核心发现

### 当前 SOTA 模型的工程短板

**Pass@1 最高仅 43.49%**（GPT-5.4），但更震撼的是 **Pass³（连续3次全对）骤降 30-50%**：

| 模型 | Pass@1 | Pass³ |
|------|--------|-------|
| GPT-5.4 (Codex) | 43.49% | 29.2% |
| Opus 4.7 (Claude Code) | 41.89% | 22.9% |
| GLM-5 (开源最佳) | 24.03% | 个位数 |
| Kimi K2.5 | 15-19% | 个位数 |

**核心结论：当前最强 AI 编程智能体是优秀的补丁工，却是糟糕的工程师。**

### 三大被低估的工程工作流

1. **Codebase Q&A**（代码库问答，124题）：深度理解陌生代码库，回答架构、运行时行为、安全问题
2. **Test Writing**（测试编写，90题）：撰写生产级测试，覆盖单元/集成/端到端
3. **Refactoring**（代码重构，70题）：在不变可观测行为的前提下重组代码

### 评估方式创新：rubric-based LLM-as-a-Judge

传统基准用 Pass/Fail 衡量功能，SWE Atlas 用专家编写的结构化打分表逐项评估：

- Codebase Q&A 平均 10.5 条 rubric
- Test Writing 平均 17.1 条 rubric
- Refactoring 平均 17.4 条 rubric + 18 条测试

**功能正确 ≠ 工程合格**：60-80% 的 Refactoring 任务能通过回归测试，但拉上 rubric 评分后立刻被腰斩。

## 工程严谨度的主要差距

前沿模型与开源模型的最大差距集中在：
- **Code Maintainability**（代码可维护性）
- **Artifact Cleanup**（旧产物清理）
- **Test Comprehensiveness**（测试全面性）

## 相关概念

- [[harness-engineering]] — Harness 五子系统是解决工程严谨度问题的工程基础设施
- [[agent-production-evaluation]] — Agent 生产评估三层次（上线前行为测试 vs 运行中链路观测 vs 事故后组织指标）
- [[claude-code-insights]] — Claude Code 内置会话分析可辅助工程能力复盘
- [[test-time-compute-scaling]] — 推理时计算扩展与 Agent 评估的关系

## 标签

#SWEAtlas #代码基准 #工程能力 #ScaleAI #AI编程 #Agent评测