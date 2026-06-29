---
title: H-RePlan
created: 2026-06-20
updated: 2026-06-20
type: entity
tags: [agent, architecture, multi-agent, reasoning, benchmark]
sources: [raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]
confidence: high
---

# H-RePlan

## Overview

H-RePlan（Hierarchical RePlan）是上海交大、东南大学、清华大学联合提出的**跨设备 Agent 分层恢复框架**，针对多设备计算机操作任务中的动态运行时故障问题。^[raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]

## Core Problem

现有跨设备 Agent 系统（如 CRAB、UFO3）将每个设备暴露为单一执行策略（GUI-only 或 CLI-only），恢复粒度粗糙：
- 失败时只能：重试同一策略、跨设备分配、或修改全局计划
- 无法区分**可在当前设备内修复**的失败 vs **需要跨设备重规划**的失败

实际场景举例：报销流程需要手机收集发票 → 笔记本整理 → 上传系统。现有系统可能在 Linux 设备上 CLI 克隆失败时直接卡住，无法切换为浏览器下载。^[raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]

## Architecture

### 两层分离

**设备层 — Strategy Planner**
- 为每个设备维护 API/CLI/GUI 三种执行策略
- 策略选择器在策略级失败时**本地恢复**（切换策略或修改指令）
- 三种状态：Create（创建执行计划）→ Progress Check（检查进度）→ Replan（策略失败时重规划）

**系统层 — Orchestrator**
- 维护全局跨设备计划链
- 只在收到**需要跨设备重规划**的故障信号时才介入
- 操作：重试修改后的指令、路由到其他设备、插入恢复子任务、保留部分进度、宣布全局失败

### 关键抽象：CLFE（Cross-Layer Failure Event）

故障信息在设备层→系统层传递时，二进制信号太弱（全量 trace 太重）。CLFE 是紧凑的 5 元素抽象：
1. 失败子任务身份
2. 源设备
3. 分类故障类型
4. 本地策略尝试摘要+观察
5. 必须升级的原因

CLFE 让 Orchestrator 能够判断：是同一设备重试，还是跨设备分配，而无需携带完整 Agent trace。^[raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]

### 三种策略执行器

| 策略 | 能力 | 适用场景 |
|------|------|----------|
| API Agent | 结构化服务访问 | 可靠高效的接口调用 |
| CLI Agent | 本地计算/文件系统 | shell 脚本、系统操作 |
| GUI Agent | 用户界面交互 | 无结构化 API 时的兜底 |

## HeraBench 基准

同论文提出的故障注入基准：
- **23 个种子任务** → 扩展为 **174 个评估变体**
- 四设备环境：2× Linux + 2× Android
- 故障类型：局部故障（禁用某策略但留同设备替代方案）、全局故障（同设备所有路径均不可用）、混合故障

关键发现：本地故障中，**不在早期升级**（设备层内解决）比立即升级的完成率高 76.81% vs 68.89%。^[raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]

## 实验结果

| 系统 | 完成率 | 指令遵从率 | 完全通过率 | Token/Episode | Token/完美通过 |
|------|--------|-----------|-----------|---------------|--------------|
| CRAB-GUI | 2.16% | 9.30% | 0.00% | 547K | ∞ |
| UFO3-GUI | 46.86% | 56.67% | 13.79% | 1.45M | 10.51M |
| UFO3-API | 61.05% | 67.81% | 0.00% | 322K | ∞ |
| **H-RePlan** | **75.84%** | **77.72%** | **36.78%** | 711K | **1.93M** |

H-RePlan 完美通过率 36.78% vs UFO3-GUI 的 13.79%；Token 成本降低 5.44×（10.51M→1.93M）。^[raw/papers/2026-06-20-h-replan-hierarchical-cross-device-agent.md]

## Ablation 消融

- 去全局重规划：完成率↓34.59pp（最严重退化）
- 去 Strategy Planner：完全通过率从 36.78%→11.49%（违反指令问题更严重）
- 去 CLFE：32.18%（仍有效但下降）
- 去 API 策略：21.84%（API 提供高效可靠访问）

## Related

- [[harness-engineering]] — H-RePlan 的分层恢复思想与 Harness 的 PE→CE→HE 演进一脉相承
- [[test-time-compute-scaling]] — 都研究推理时计算资源的有效利用
- [[agent-drift-multi-agent-systems]] — 多智能体系统长期运行的行为退化问题
- [[autolab-long-horizon-agent-benchmark]] — 长时 Agent 评估基准
- [[recursive-agent-harnesses]] — 递归 Agent Harness 模式
