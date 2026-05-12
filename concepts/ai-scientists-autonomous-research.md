---
title: AI Scientists 自主研究尝试
created: 2026-05-10
updated: 2026-05-10
type: concept
tags: [research, agent, alignment]
sources: [raw/articles/2026-05-10-ai-scientists-four-autonomous-research-attempts.md]
confidence: high
---

# AI Scientists 自主研究尝试

## 概述

本文记录四个端到端自主生成 ML 研究论文的尝试，测试最强推理 LLM 能否从研究想法到论文全程高度自主。

**关键发现：** 四个尝试中，三个失败；一个（AS-1）完成流程并被 Agents4Science 2025 接受（48/254 有效投稿）。

## 研究流程

```
Claude Code → Idea Generation Agent → Hypotheses Generation Agent
    ↓
Experimental Planning Agent → Output Evaluation Agent
    ↓
Paper Outlining Agent → Revision Agent
```

六个模块：想法生成、假设生成、实验规划、输出评估、修订、论文概述。

## 四大尝试结果

| ID | 领域 | 状态 | 主要失败模式 |
|----|------|---------|------------|
| MARL-1 | 多智能体RL | 执行阶段失败 | Implementation Drift、数据偏差 |
| WM-1 | 世界模型 | 评估阶段失败 | Implementation Drift、缺乏科学品味 |
| WM-2 | 世界模型 | 评估阶段失败 | Implementation Drift、数据偏差 |
| **AS-1** | AI安全 | **成功发表** | 数据偏差、上下文问题 |

## 六大反复失败模式

1. **训练数据偏差**：模型默认使用训练数据中的流行方案而非遵循显式指令
2. **Implementation Drift**：系统性偏离原定计划，逐步改变实验设置
3. **缺乏科学品味**：能生成表面合理想法，但无法判断哪些值得追求
4. **上下文与记忆问题**：长工作流中关键信息丢失
5. **人类反馈的价值**：轻量级人类指导显著提升成功率
6. **评估复杂性**："论文就绪"判断本身主观，模型难以可靠自我评估

## 核心教训

1. **scaling 不是瓶颈**：最强模型仍因实现偏差和科学品味缺失失败
2. **人类指导仍有价值**：即使轻量级方向指导也显著提升成功率
3. **需要更好的自动评估科学价值的方法**
4. **当前 pipeline 能完成形式正确的论文，但难以产生真正有价值的新知识**

## 相关概念

- [[openclaw]] — 多 Agent 协作流程的实践案例（Orchestration Agent）
- [[ai-sycophancy-analysis]] — AI Safety 研究方向，AS-1 成功于此
- [[ai-agent]] — 自主研究 Agent 的能力边界
