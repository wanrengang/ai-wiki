---
title: Agentic Code Reasoning — 半形式化代码推理
created: 2026-05-15
updated: 2026-05-15
type: concept
tags: [agent, coding, reasoning]
sources: [raw/articles/2026-05-15-agentic-code-reasoning-2603.01896.md]
confidence: high
---

# Agentic Code Reasoning — 半形式化代码推理

## 概述

Agentic Code Reasoning = LLM agent 在不做代码执行的情况下，导航代码库文件、追踪依赖关系、执行深度语义分析的能力。

核心问题：
- LLMs 可以对代码行为做出无明确理由的断言
- 非结构化思维链允许跳过情况或做出无支撑声明
- 形式化验证方法需要不切实际地转换为形式语言

## 核心方法：Semi-formal Reasoning

结构化提示方法，要求 agents 构造：
- **显式前提**（Explicit premises）
- **执行路径追踪**（Execution path traces）
- **形式化结论**（Formal conclusions）

结构化格式作为"证书"——agents 无法跳过情况或做出无支撑声明。

## 关键结果

### Patch Equivalence（判断两个补丁是否等价）
| 数据集 | Standard | Semi-formal |
|--------|----------|-------------|
| 精选170例 | 78.2% | **88.8%** |
| 真实世界Agent生成Patch | 86.0% (Opus-4.5) | **93.0%** |

→ 93% 准确率使 execution-free RL 奖励信号成为可能

### Fault Localization（Defects4J，50 bugs）
- Standard Top-5 Any: 69.4%
- **Semi-formal Agentic Top-5 Any: 88.4%**

### Code Question Answering（RubberDuckBench）
- Standard: 78.3% → **Semi-formal: 87.0%**

## 关键案例：Name Shadowing（django-13670）

判断两个修复两位年份格式的补丁是否等价：

Patch 1: `return format(self.data.year, "04d")[-2:]`
Patch 2: `return '%02d' % (self.data.year % 100)`

标准推理（错误）：两者都修复了同一bug，结果相同。
半形式化推理（正确）：追踪执行路径 → 发现 `format` 被 Django 模块级函数 shadow → Patch 1 raise `AttributeError`，Patch 2 成功。

## 相关概念

- [[openclaw]] — 代码库导航与 agent 工作流相关
- [[ai-agent]] — Agent 能力边界扩展
- [[test-time-compute-scaling]] — 推理时计算 scaling 与 reasoning 范式

## 作者

Shubham Ugare et al., Meta

## 参考

- 论文：https://arxiv.org/abs/2603.01896
