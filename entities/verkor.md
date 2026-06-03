---
title: Verkor
created: 2026-06-01
updated: 2026-06-01
type: entity
tags: [company, model, architecture, agent, engineering]
sources: [raw/articles/2026-05-24-verkor-design-conductor-ai-chip-design.md]
confidence: high
---

# Verkor

## 概述

Verkor 是一家 AI 芯片设计初创公司，推出了 **Design Conductor**——一套完全基于 LLM Agent 的芯片设计编排框架，能够在没有工程师介入的情况下完成从需求规格到 GDSII 版图的全流程。

## 关键事实

- **创始人：** Suresh Krishna
- **核心产品：** Design Conductor（LLM harness 框架）+ VerCore（自主设计的 RISC-V CPU）
- **技术突破：** 219 个英文单词的需求描述 → 12 小时 → 7nm GDSII 版图，业界首次 AI 走通完整芯片设计流程
- **VerCore 规格：** RV32I + ZMMUL，5 级流水线，顺序单发射，CoreMark 3261，时钟频率 1.48GHz，面积 2809μm²（ASAP7 7nm PDK）
- **算力消耗：** 数百亿个 token（设计一颗简单 CPU）
- **对标性能：** 相当于 2011 年 Intel Celeron SU2300（嵌入式基准，非旗舰处理器对比）

## 团队观点

核心观点来自工程副总裁 David Chin：
> 「我们在用算力换经验。」—— AI Agent 能跑通流程，不等于它真正理解硬件设计，用试错次数弥补人类工程师靠多年积累形成的直觉。

## 愿景

未来 100 人以上、花 18-36 个月才能完成一颗芯片的团队，将能够同时探索多个从概念到 GDSII 的设计方案，整体流片周期压缩到 **3-6 个月**。人的角色从「操作 EDA 工具」转向「目标设定和架构引导」。

## 局限性（论文坦承）

1. **架构决策绕远路** — AI 有时会先去想要不要加深流水线，而有经验的工程师一眼就知道先找更简单的原因
2. **代码理解偏差** — AI 把硬件描述语言的并行逻辑当成普通顺序程序理解，导致调试低效
3. **规格必须极度精确** — 少写一个 CPI 约束，AI 可能默默交出一颗分支处理很差的处理器

## 相关概念

- [[harness-engineering]] — Design Conductor 本身就是一个大型 LLM Harness，是 Harness Engineering 的典型案例
- [[gram-generative-recursive-reasoning]] — Bengio 的 GRAM 是另一种递归推理方案，两者都探索推理效率
- [[agent-engineering]] — Agent 进入真实生产流程后的工程挑战

## 标签

#Verkor #DesignConductor #AI芯片设计 #RISC-V #HarnessEngineering #EDA #Agent