---
title: MLEvolve — 自我演化 MLE Agent 框架
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [agent, architecture, training, self-evolution, automation]
sources: [raw/articles/2026-06-08-mlevolve-self-evolving-mle-agent.md]
confidence: high
---

# MLEvolve — 自我演化 MLE Agent 框架

## 概述

**MLEvolve**（arXiv:2606.06473，上海 AI Lab + 华东师大）是首个面向端到端机器学习算法发现的**自我演化多智能体框架**，在 MLE-Bench 12 小时预算下达到 **65.3% 平均奖牌率**，超越所有现有方法，且在数学算法优化任务上超越 AlphaEvolve。

## 核心挑战

现有 MLE Agent 面临三大阻碍长期优化的瓶颈：

1. **分支信息隔离**：树搜索各分支信息不互通，成功策略无法跨轨迹迁移
2. **无记忆搜索**：每步决策孤立进行，无法复用历史尝试的经验
3. **缺乏层级控制**：每次迭代全量重写，迭代效率低、修改不可控

## 三大核心组件

### 1. Progressive MCGS（渐进式蒙特卡洛图搜索）

将 MCTS 扩展为图搜索结构：
- **主边 E_T**：保留父子生成顺序，用于选择和反向传播
- **引用边 E_ref**：跨分支连接，将其他分支节点信息注入当前节点，实现**跨分支知识流动**和组合迁移
- **熵驱动渐进探索调度**：早期广度探索 → 后期专注利用（exploitation）

### 2. Retrospective Memory（回溯记忆）

- **冷启动知识库**：精选领域知识用于初始化
- **动态全局记忆**：搜索过程中**自动累积**任务特定经验并检索复用，无需额外 LLM 调用进行显式反思

### 3. 分层规划 + 自适应代码生成

将战略规划与代码实现解耦，根据搜索状态自适应选择三种模式：
- **全量重写**（full rewrite）
- **逐步修改**（stepwise）
- **diff 编辑**（diff-based editing）

## 实验结果

- MLE-Bench 12 小时（标准一半）：**65.3% 平均奖牌率**，SOTA
- 数学算法优化任务：超越 AlphaEvolve 和 AlphaEvolve-v2
- 跨域泛化能力强

## 与其他框架的关系

MLEvolve 与以下框架形成自我演化 Agent 演进脉络：

- [[moss-source-level-self-evolution]] — MOSS 专注源代码级自我演化，解决文本层无法触达的结构性故障；MLEvolve 扩展到端到端 ML 算法发现层面
- [[harness-engineering]] — Harness 解决单次任务中 Agent 的工程约束问题；MLEvolve 解决长期多次迭代中的自我演化问题（信息隔离+记忆累积+层级控制）
- [[agent-drift-multi-agent-systems]] — Agent Drift 研究多智能体长期运行的行为退化；MLEvolve 通过 Retrospective Memory 和分层规划实现**稳定**的长期演化
- [[capability-evolver]] — Capability Evolver 关注 Agent 能力演化；MLEvolve 提供了更具体的 MLE 场景自我演化实现路径

## 来源

- 论文：https://arxiv.org/abs/2606.06473
- 代码：https://github.com/InternScience/MLEvolve
