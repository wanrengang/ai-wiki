---
title: AI Scientists 自主研究尝试
created: 2026-05-10
updated: 2026-06-01
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
- **AS-1** | AI安全 | **成功发表** | 数据偏差、上下文问题 |

## 2026-05-29 更新：陈德里综述

DeepSeek 研究员陈德里（Deli Chen）发布 46 页综述论文 *From Copilots to Colleagues: A Survey of Autonomous Research Agents*，由 AI Agent 深度参与完成（1% 人类，99% AI）。核心贡献：

- **L1-L5 自主等级分类**：L1 自动补全 → L5 自主设定研究议程，当前最强系统处于 L4
- **四种架构模式**：单智能体循环、多智能体协作（MetaGPT 67%→100%）、层级编排（Claude Code）、工具增强执行（ChemCrow 30%→75%）
- **六大未解难题**：认知循环陷阱、上下文窗口限制、新颖性评估、可重现性危机、安全伦理、成本可及性

产出数据：6 轮迭代、108 轮 Agent 交互、64.8 万 tokens、6 天、95+ 篇参考文献、人类实际"动脑"时间不到 2 小时。

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
- [[muse-autoskill-agent]] — 字节跳动自进化 Agent 框架，Skill 全生命周期管理
- [[codex-goal-mode-ai-science]] — Codex Goal Mode，1小时56分完成博士80小时任务

## 2026-06-01 更新：AutoSci — 全流程科研 Agent 系统 (arXiv 2605.31468)

北京大学发布 AutoSci，内存中心化科研 Agent 系统，四模块架构：

- **SciMem**：结构化科研记忆，分层（长期知识库 vs 项目级活跃记忆）
- **SciFlow**：Harness 驱动执行，5 阶段生命周期（文献→想法→实验→写作→反驳）
- **SciDAG**：DAG 形多 Agent 算子，增强复杂推理阶段
- **SciEvolve**：反馈驱动自我演化，修改自身 skills 和工作流

**关键贡献：** 首次同时具备完整生命周期支持 + 执行 Harness + 结构化持久记忆 + 系统自演化。对比 AI Scientist 系列、Agent Laboratory、ARIS、NORA 等，均缺失至少一项。

AutoSci → [[ai-scientists-autonomous-research]]，补充全流程 + 自演化能力这一环
