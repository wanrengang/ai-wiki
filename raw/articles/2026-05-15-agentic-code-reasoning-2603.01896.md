---
source_url: https://arxiv.org/abs/2603.01896
ingested: 2026-05-15
sha256: 2cfa038b9efe7967847e8c5e13894f3ea027839ea0e8619e8d31896b1dc47640
---
# Agentic Code Reasoning

arXiv:2603.01896

## Core Concept

Agentic Code Reasoning = LLM agent在不做代码执行的情况下，导航代码库文件、追踪依赖关系、执行深度语义分析的能力。

核心方法：Semi-formal Reasoning（半形式化推理）
- 要求agents构造：显式前提、执行路径追踪、形式化结论
- 结构化格式作为"证书"——agents无法跳过情况或做出无支撑声明

## 关键评估结果

### Patch Equivalence（170个精选例子）
- Standard准确率：78.2%
- Semi-formal准确率：88.8%（提升~10pp，错误减少近一半）

### 真实世界Agent生成Patch
- Opus-4.5 Single Call: 86.0%
- Opus-4.5 Agentic (Semi-formal): 93.0%
→ 93%准确率使execution-free RL奖励信号成为可能

### Fault Localization（Defects4J，50 bugs）
- Standard Top-5 Any: 69.4%
- Semi-formal Agentic Top-5 Any: 88.4%

### Code Question Answering（RubberDuckBench）
- Opus-4.5 Agentic (Semi-formal): 87.0%（标准78.3%）

## 关键发现：Name Shadowing（django-13670）

Patch 1: `return format(self.data.year, "04d")[-2:]`
Patch 2: `return '%02d' % (self.data.year % 100)`

标准推理（错误）：两者都修复了同一bug，year 476时结果相同。
半形式化推理（正确）：追踪执行路径 → 发现format被Django的模块级函数shadow → Patch 1 raise AttributeError，Patch 2成功。

## 作者

Shubham Ugare et al., Meta

原文：https://arxiv.org/abs/2603.01896