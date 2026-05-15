---
title: Lilian Weng Why We Think
created: 2026-05-13
updated: 2026-05-13
type: concept
tags: [reasoning, inference, optimization, research]
sources: [raw/articles/2026-05-13-lilian-weng-why-we-think.md]
confidence: high
---

# Lilian Weng: Why We Think

## Overview

Lilian Weng（OpenAI 研究员，知名 AI 博客 Lil'Log 作者）2025年5月发布的深度文章，系统解析大语言模型的推理（Thinking）与思考（Reasoning）机制。

## Core Topics

### Self-Correction in LLM Reasoning

**朴素的自纠正常导致更差表现** — 模型自我改进需要外部反馈。反馈来源：
- 匹配 ground truth
- 启发式方法和任务特定指标
- 代码问题的单元测试结果（Shinn et al. 2023）
- 更强的模型作为纠正器（Zhang et al. 2023）

### Scaling CoT Reasoning Path Length

**Budget Forcing** 技术（Zhang et al. 2025）：
- **延长**：强制追加 "wait" 一词以延长推理过程
- **缩短**：通过追加 end-of-thinking token 或 "Final Answer:" 终止模型的思考过程

### Monitoring Reasoning Models

监控推理模型的思维链（CoT）可有效检测模型错误行为，如 **reward hacking**，甚至可以让弱模型监控强模型。

### Adaptive Computation

结合 Transformer 中的自注意力与 RNN 中的循环机制，使用 **Adaptive Computation Time（Graves, 2016）** 动态调整计算步数。

## Key References

| 论文 | 作者 | 年份 | 贡献 |
|------|------|------|------|
| Budget Forcing | Zhang et al. | 2025 | CoT 推理路径长度缩放 |
| Reflexion | Shinn et al. | 2023 | 语言 Agent 言语强化学习 |
| Learning to Learn at Test-Time | Zhang et al. | 2023 | 测试时学习 |
| Adaptive Computation Time | Graves | 2016 | 动态计算步数 |

## Related Concepts

- [[test-time-compute-scaling]] — Budget Forcing 是 Test-Time Compute Scaling 的具体技术之一
- [[ai-agent]] — Self-correction 是 AI Agent 的核心能力之一
- [[tool-hallucination-reasoning-trap]] — 监控推理模型错误行为与推理陷阱研究相关
- [[harness-engineering]] — 自我纠正在 Agent Harness 框架中的角色
- [[fine-tuning-with-trl]] — Reflexion 等技术与 TRL 库的强化学习训练有潜在关联
