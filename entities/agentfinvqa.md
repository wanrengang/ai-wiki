---
title: AgentFinVQA
created: 2026-06-22
updated: 2026-06-22
type: entity
tags: [model, agent, benchmark, research]
sources: [raw/papers/2026-06-22-agentfinvqa-multi-agent-financial-chart-qa.md]
confidence: high
---

# AgentFinVQA

## Overview

AgentFinVQA 是 Vector Institute 提出的多智能体金融图表问答管道，核心贡献是将**可审计性（auditability）与本地部署（on-premise）**结合到金融图表 QA 任务中，且不牺牲精度。arXiv:2606.19782。

## Key Facts

- **Authors:** Aravind Narayanan, Shaina Raza (Vector Institute)
- **Published:** 2026-06
- **Benchmark:** FinMME (Luo et al., 2025)
- **Backbone:** Gemini-3 Flash (proprietary) / Qwen3.6-27B-FP8 (open-weights, single A100)
- **Code:** Released for reproducible evaluation

## Architecture

六阶段管道（固定顺序，条件门控跳过）：

```
Plan → OCR → Ground → Colour-Area → Inspect → Verify
```

| Stage | Role | Key Detail |
|-------|------|------------|
| Planner | 文本 LLM，不看图，输出结构化 JSON 检查计划 | MCQ 多选独立验证指令 |
| OCR Reader | 单次 VLM 调用转录所有可见文本 | 图表类型、轴标签、图例、数据标注 |
| Legend Grounder | VLM 将图例映射到视觉属性（RGB、线型） | 合规检查拦截 13.2% 的无引用回答 |
| Colour-Area Tool | HSV 颜色掩码像素计数（确定性，无 GPU） | 仅在条形/饼图 + 比较关键词时触发 |
| Vision Agent | CrewAI 编排，执行检查计划 | 强制选择重试消除 MCQ 放弃率 |
| Verifier | 第二路独立 VLM 审计草稿答案 | 置信度门控（<0.75 降级为确认），68.2% vs 55.6% 精确率 |

**MEP（Model Evaluation Packet）：** 每样本每阶段记录输入、输出、工具调用、时间戳，输出为便携 JSON artifact，支持复现评估、事后错误归因和无需重跑管道的审计。

## Results

| System | Backbone | Mean Acc. | Exact | MCQ | Open Q |
|--------|----------|-----------|-------|-----|--------|
| Zero-shot | Gemini-3 Flash | 63.56% | 56.40% | 64.4% | 54.0% |
| AgentFinVQA | Gemini-3 Flash | 71.24% | 65.12% | 72.5% | 57.0% |
| **Δ** | | **+7.68 pp** | **+8.72 pp** | **+8.1 pp** | +3.0 pp |
| Zero-shot | Qwen3.6-27B-FP8 | 61.68% | 53.52% | 62.8% | 49.0% |
| AgentFinVQA | Qwen3.6-27B-FP8 | 66.52% | 60.24% | 68.1% | 48.0% |
| **Δ** | | **+4.84 pp** | **+6.72 pp** | **+5.3 pp** | ≈noise |

消融实验关键发现：
- MCQ 感知规划 + 强制选择重试：近乎消除 MCQ 放弃率
- 多选 MCQ 支持：+23.3 pp on multiple_choice
- 验证器置信度门控：确认答案 68.2% vs 修订答案 55.6%，验证器裁决可作为人类审核路由信号

错误分析：问题误解、图例混淆和提取错误占失败案例近 2/3，且是验证器最少检测到的类别。

## Deployment Value

- **数据本地化：** Qwen3.6-27B-FP8 单卡 A100-80G 即可部署，敏感金融图表不离境
- **精度代价小：** 开源权重仅比专有 API 低约 5pp（68.2% vs 71.24%）
- **可审计性：** MEP 使每个答案可溯源，满足金融监管要求

## Relationships

- 对比 [[test-time-compute-scaling]]：AgentFinVQA 的验证器本质是 Test-Time ComputeScaling 的应用——多步独立推理 + 自我纠正
- 相关 [[harness-engineering]]：AgentFinVQA 体现了 PE→CE→HE 中"约束越多元智能体干得越好"——MEP 作为结构化约束
- 相关 [[agent-drift-multi-agent-systems]]：多智能体长期运行的语义漂移问题，AgentFinVQA 的 MEP 提供了可审计轨迹可作为漂移检测基础设施
- 相关 [[ledgeragent]]：两者都解决 Agent 工具调用中的状态管理问题——LedgerAgent 专注状态 Schema，AgentFinVQA 专注状态轨迹可审计性
- 相关 [[containment-gap-agentic-framework-security]]：都关注 Agent 框架的安全合规，AgentFinVQA 的 MEP 是一种确定性合规记录机制
- 涉及 [[rag]] 技术：OCR Reader 将图表转为结构化文本，本质是 Chart-to-Table 的 RAG pipeline
- 涉及 [[agent-memory-systems-characterization]]：AgentFinVQA 的 MEP 是一种显式记忆形式，每个答案都是可审计的记忆轨迹

## Tags

- model / agent / benchmark / research