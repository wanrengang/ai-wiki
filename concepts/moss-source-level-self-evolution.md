---
title: MOSS — Source-Level Self-Evolution
created: 2026-05-22
updated: 2026-05-22
type: concept
tags: [agent, self-evolution, openclaw, code, architecture]
sources: [raw/articles/2026-05-22-moss-self-evolution-source-level-rewriting.md]
confidence: high
---

# MOSS — Source-Level Self-Evolution

## 概述

**MOSS**（来自论文 arXiv:2605.22794）提出了一个关键洞察：当前的自进化 Agent 系统都将进化限制在"文本可修改层"（skill files、prompt 配置、memory schemas、workflow graphs），而 Agent 的 harness（路由逻辑、hook 顺序、状态不变式、分发逻辑）存在于代码层，永远无法通过文本层触达。

MOSS 主张：**源代码级适应是本质更广泛的进化媒介**，它是图灵完备的，是所有文本可修改范围的严格超集，且不受 long-context drift 影响。

## 核心论点

1. **文本层盲区**：路由逻辑、hook 顺序、状态不变式、分发逻辑都在代码层，文本层根本无法修改这些结构性故障
2. **源代码的优势**：
   - 图灵完备（文本层不是）
   - 是所有文本可修改范围的严格超集
   - 确定性生效（不依赖 base model 遵从性）
   - 不受 long-context drift 侵蚀
3. **评估结论**：在 OpenClaw 上，MOSS 将四任务平均 grader score 从 0.25 提升至 0.61，单次循环无需人工干预

## 架构

```
Production Failure Evidence (自动采集)
         ↓
  [多阶段确定性管道]
         ↓
  ┌─ 阶段1：证据整理
  ├─ 阶段2：代码修改（委托给外部 coding-agent CLI，MOSS 保留阶段排序和判决权）
  ├─ 阶段3：候选判决
  └─ 阶段4：验证
         ↓
  Ephemeral Trial Workers（重放验证）
         ↓
  User-Consent-Gated 部署 + Health-Probe Rollback
```

关键特点：
- **外部 coding-agent CLI 是可插拔的**：可以用 Claude Code、OpenCode 等
- **MOSS 保留控制权**：阶段排序和 verdicts 由 MOSS 决策，不委托给外部 agent
- **用户同意门控**：部署前需要用户确认
- **健康探测回滚**：自动检测并回滚失败的更新

## 与 Library Drift（Ratchet）的关系

MOSS 和 Ratchet（arXiv:2605.19576, "Library Drift"）是自进化 Agent 的两个互补维度：

| | MOSS | Ratchet |
|---|---|---|
| **问题** | 文本层无法触达结构性故障（harness 代码） | 无限制技能累积导致检索退化、假阳性注入 |
| **解决方案** | 源代码级自重写 | 成果驱动的技能退休 + 有界 active-cap |
| **评估** | OpenClaw 四任务从 0.25→0.61 | MBPP+ hard-100 100轮后 pass@1: 0.258→0.584 |

两者共同构成自进化 Agent 的完整治理框架：**MOSS 解决进化深度，Ratchet 解决进化广度**。

## 与 [[capability-evolver]] 的关系

[[capability-evolver]] 是一个 meta-skill，让 Agent 检查自身运行历史并自主改进。MOSS 将这个思路扩展到源代码层——不仅改进 skill 和配置，还改进 Agent 的 harness 本身。

关键区别：
- Capability Evolver：进化文本可修改的 artifacts
- MOSS：进化代码层——harness、路由逻辑、hook 顺序

## 与 [[harness-engineering]] 的关系

MOSS 是 [[harness-engineering]] "HE（Harness Engineering）"阶段的实践案例——通过源代码级适应直接改进 Agent 的编排基础设施，而非依赖文本层的间接改进。

## 相关页面

- [[openclaw-ecosystem]] — MOSS 评估基于 OpenClaw
- [[capability-evolver]] — 元技能自进化，文本层局限
- [[harness-engineering]] — HE 演进，harness 是自进化的基础设施
- [[agent-drift-multi-agent-systems]] — 多智能体长期运行退化问题
- [[scheming-propensity-llm-agents]] — 自进化 Agent 的激励扭曲风险
