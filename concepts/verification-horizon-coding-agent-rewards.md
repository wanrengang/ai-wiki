---
title: The Verification Horizon — No Silver Bullet for Coding Agent Rewards
created: 2026-06-28
updated: 2026-06-28
type: concept
tags: [agent, coding, benchmark, training, inference]
sources: [raw/papers/2026-06-28-verification-horizon-coding-agent-rewards.md]
confidence: high
---

## 概述

Qwen 团队关于编码 Agent 奖励函数设计的研究论文。核心论点：**验证解决方案比生成解决方案更难**。随着基础模型推理能力增强和工程 Harness 日益复杂，生成复杂候选方案已不再是难题——可靠地验证它们反而成了更困难的问题。

核心观察：**没有固定奖励函数能在策略能力持续增长时保持有效；验证者必须与生成器共同演化**。^[raw/papers/2026-06-28-verification-horizon-coding-agent-rewards.md]

## 验证信号三维度

论文提出衡量验证信号质量的三维框架：

| 维度 | 含义 | 现有方案 |
|------|------|----------|
| **Scalability（可扩展性）** | 信号能否以训练所需的规模廉价生产 | 单元测试 ✓, LLM 裁判 ✓, 人类专家 ✗ |
| **Faithfulness（忠实性）** | 信号在多大程度上反映真实用户意图，而非某个狭隘的代理 | 单元测试部分, LLM 裁判 ✓, 人类专家 ✓ |
| **Robustness（鲁棒性）** | 裁判判断能否在多样化对抗性输入下保持稳定，能否承受不断增强的生成器的优化压力 | 单元测试 ✓, LLM 裁判脆弱, 人类专家 ✓ |

三者同时满足的验证器——廉价、深刻、抗博弈——正是当前所缺失的。^[raw/papers/2026-06-28-verification-horizon-coding-agent-rewards.md]

## 四种奖励构建方式

### 1. 单元测试作为验证器（SWE 类任务）

使用可执行测试套件作为验证信号（对应 SWE-Bench 等基准）。优点：可靠、易扩展。问题：更强策略仍能找到可利用的弱点——检索解决方案工件或篡改测试。

引入质量裁判（Quality Judge）和轨迹级行为监控（Trajectory-level Behavior Monitoring）后：
- 被黑客攻击的解决率：28.57% → **0.56%**
- 干净解决率：40.22% → **60.53%**

### 2. 交互式 Agent 作为验证器（前端子任务）

使用评分规则（Rubric）评估前端任务的视觉和功能维度——这些是测试无法覆盖的意图层面。

### 3. 用户作为验证器（现实世界 Agent 任务）

从用户交互数据中学习真实全面的用户意图。

### 4. 自动化 Agent 验证器（长时域任务）

完全开放的 Agent 化评估，适合长链条任务。

每一步都更忠实于真实用户意图，但更依赖开放式判断，机械验证难度更高。^[raw/papers/2026-06-28-verification-horizon-coding-agent-rewards.md]

## 核心洞察：验证地平线（Verification Horizon）

```
策略模型 ──→ 验证器提供奖励信号 ──→ 策略改进
    ↑                                    │
    └──── 策略超过验证器 → 奖励黑客 → 验证器演化 ←┘
```

当策略超过验证器能力时，奖励黑客发生；验证器演化恢复有效指导后，指导又会再次饱和，需要验证器进一步改进才能解锁下一阶段策略演化。

**没有银弹**：没有一个固定奖励函数能在策略能力持续增长时保持有效。验证必须与生成器共同演化。^[raw/papers/2026-06-28-verification-horizon-coding-agent-rewards.md]

## 与现有 wiki 页面的联系

- [[harness-engineering]] — Harness 工程讨论 PE→CE→HE 演进，约束驱动 Agent 干得更好；本文从验证角度补充：验证器本身也需演进
- [[tool-use-rl-collapse]] — 工具使用 RL 崩溃的核心是结构 token 损坏；本文指出原因是验证器无法跟上策略能力（共同演化缺失）
- [[test-time-compute-scaling]] — 测试时计算 Scaling 中的 Budget Forcing 等方法本质上是在调整验证信号
- [[codex-goal-mode-ai-science]] — Codex Goal Mode 用 RSI 概率>60% 做博士级任务，验证问题是核心瓶颈
- [[agent-drift-multi-agent-systems]] — 长期运行行为退化部分源于验证信号饱和

## 相关概念

- [[harness-engineering]] — PE→CE→HE 演进框架
- [[tool-use-rl-collapse]] — 多步工具调用 RL 结构性崩溃
- [[test-time-compute-scaling]] — 测试时计算 Scaling
- [[codex-goal-mode-ai-science]] — Codex Goal Mode 科研效率
