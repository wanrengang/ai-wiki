---
title: Verkor Design Conductor
created: 2026-06-05
updated: 2026-06-05
type: entity
tags: [company, agent, chip-design, eda]
sources: [raw/articles/2026-05-24-verkor-design-conductor-chip-design.md]
confidence: high
---

# Verkor Design Conductor

## 概述

Verkor 是 AI 芯片设计初创公司，其系统 **Design Conductor** 是全球首个完整跑通芯片设计全流程（从需求到 GDSII 版图）的 LLM Agent 系统。

## 关键事实

- **Design Conductor**：LLM Agent 框架，接收 219 词需求描述，输出基于 ASAP7 7nm PDK 的 tape-out ready GDSII 版图文件
- **VerCore CPU**：Design Conductor 设计的 RISC-V CPU 核心，RV32I + ZMMUL 扩展，5 级流水线，顺序单发射
- **性能**：CoreMark 3261，时钟频率 1.48GHz，面积 2809 μm²（≈2011 年 Intel Celeron SU2300）
- **流程覆盖**：需求分析 → 架构设计 → RTL 编码 → 功能验证 → 时序收敛 → 物理版图（端到端无人介入）
- **状态**：仅完成仿真验证，尚未实际流片（ASAP7 是学术 PDK，非台积电/三星量产工艺）

## 技术架构

Design Conductor 由以下子 Agent 协作：
- **Design Planning Agent**：需求分析、微架构设计
- **Design Review Agent**：逐场景方案审查
- **Module Implementation Subagent**：模块实现、测试台生成、模块级测试
- **System Integration Agent**：RTL 整合、端到端系统测试
- **Root Cause Analysis Subagent**：从 VCD 波形定位根因（如流水线 flush 缺陷）
- **PPA Closure Subagent**：时序/面积/功耗优化

## 意义

这是芯片行业几十年历史上，第一次有 AI 走完了从需求到版图的全流程。虽然 VerCore 性能停留在 2011 年水平，但 **这条路的打通** 比跑分更重要。传统芯片设计需 4 亿美元+、18-36 个月、数百人团队，Design Conductor 未来有望将周期压缩至 3-6 个月。

## 相关链接

- [[harness-engineering]] — Design Conductor 本质是一个 LLM harness
- [[agent-production-evaluation]] — Agent 进入真实生产流程的评估挑战
- [[ai-scientists-autonomous-research]] — AI 自主科研的长 horizon 迭代问题
- [[verkor]] — （归档：此页面为 Verkor 公司主页面）