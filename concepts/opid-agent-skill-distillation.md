---
title: OPID - On-Policy Skill Distillation for Agentic Reinforcement Learning
created: 2026-06-27
updated: 2026-06-27
type: concept
tags: [agent, training, rl, self-evolution, architecture]
sources: [raw/papers/2026-06-27-opid-on-policy-skill-distillation-agentic-rl.md]
confidence: high
---

## 概述

OPID（On-Policy Skill Distillation）是清华大学等机构提出的**面向 Agentic RL 的 On-Policy 技能蒸馏框架**，来自 Tsinghua/Zhejiang/CUHK/NTU/Tongji。核心创新：从已完成 on-policy 轨迹中直接提取技能监督信号，无需外部技能记忆库或特权上下文。

## 核心问题：稀疏奖励 vs 密集监督

**Outcome-based RL**（以结果为奖励）为语言 Agent 提供稳定优化骨架，但：
- 稀疏的轨迹级奖励无法指导哪些中间决策应强化、哪些应抑制
- 多轮交互中，当前 policy 产生的状态分布与外部技能记忆不匹配

**现有 On-Policy 变体**依赖外部技能记忆或检索到的特权上下文：
- 维护成本高
- 状态分布可能与当前 policy 不匹配

## OPID 核心设计

### 1. 分层技能表示（Hiearchical Skills from Trajectory Hindsight）

将轨迹 hindsight 表示为两层技能：

- **Episode-level skills（回合级技能）**：捕获全局工作流或失败规避规则
- **Step-level skills（步级技能）**：捕获关键时刻的局部决策知识

### 2. Critical-First Routing（关键优先路由）

```
关键时刻识别 → 使用 step-level skill
否则 → 默认 fallback 到 episode-level skill
```

选中的技能注入交互历史，让旧 policy 在原始上下文和技能增强上下文中对相同采样的 response 重新评分。

### 3. Token级自蒸馏优势

log 概率差产生 **token 级自蒸馏优势**，与 outcome advantage 结合用于策略优化。

```
RL 为主训练目标 + 密集的、分布匹配的 hindsight 监督
```

## 实验结果

在 ALFWorld、WebShop、Search-based QA 三个标准 Agent 基准上：

- **OPID 达到最强平均性能**（ALFWorld + WebShop）
- 在 Search-based QA 保持竞争力
- 超越 outcome-only RL 和现有技能蒸馏基线

## 与现有工作的关系

OPID 与 [[on-policy-distillation-thunlp]]（清华 THUNLP）的关联：
- 都研究 On-Policy Distillation，但 OPID 专注于 **Agentic RL** 场景，THUNLP 关注思维模式一致性
- OPID 的创新在于从轨迹 hindsight 中自动提取，无需外部技能库；THUNLP 侧重 OPD 法则

与 [[tool-use-rl-collapse]] 的关联：
- 都是 RL 训练 Agent，但关注点不同：Tool-Use RL 关注多步工具调用的灾难性崩溃；OPID 关注如何在 RL 框架内加入密集技能监督

与 [[harness-engineering]] 的关联：
- OPID 本质上是一种新型 Harness 设计——用分层技能作为 RL 的密集奖励塑形机制

与 [[mlevolve-self-evolving-mle-agent]] 的关联：
- MLEvolve 通过 Proactive MCGS + Retrospective Memory 实现自演化；OPID 通过轨迹 hindsight 技能实现自指导

## 核心启示

> RL 是主训练目标，技能蒸馏提供密集的、分布匹配的 hindsight 监督——两者不是替代关系，而是互补。

关键洞察：**episode-level + step-level 的分层技能**是沟通稀疏 outcome 信号和密集 token 级监督的有效桥梁，避免了外部技能库的状态分布不匹配问题。

## 相关概念

- [[on-policy-distillation-thunlp]] — 清华 THUNLP OPD 两大法则（思维一致性 + 高分≠新知识）
- [[tool-use-rl-collapse]] — 多步工具调用 RL 的灾难性崩溃
- [[harness-engineering]] — PE→CE→HE 演进，Harness 是约束越多 Agent 干得越好
- [[test-time-compute-scaling]] — RL 自纠正、CoT 推理与 OPID 的密集监督正交互补
- [[mlevolve-self-evolving-mle-agent]] — MLE Agent 自演化框架，OPID 技能蒸馏可类比为自指导
- [[self-compacting-agents]] — 轨迹压缩，OPID 的 step-level skill 可类比为压缩后的决策知识结晶
