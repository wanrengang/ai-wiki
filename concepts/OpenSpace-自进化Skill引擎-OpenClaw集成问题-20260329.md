---
title: OpenSpace — 自进化 Skill 引擎（OpenClaw 集成）
description: 港大 HUDS 团队开源的"自进化 Skill 引擎"，让 Agent 从成功和失败中学习。三机制 AUTO-FIX/AUTO-IMPROVE/CAPTURE，Benchmark 提升 4.2 倍收入、降低 46% Token。但存在强模型兼容性矛盾
type: concept
tags:
  - OpenSpace
  - OpenClaw
  - 自进化
  - Skill
  - Agent
  - MCP
created: 2026-05-14
source: https://mp.weixin.qq.com/s/5dOxUkHJ1Mti0JOvtsCjNw
author: 嘎叔Barlcky
---

# OpenSpace — 自进化 Skill 引擎

## 概述

港大 HUDS 团队开源的"自进化 Skill 引擎"，核心解决：AI Agent 从不学习进化，同样的失败重复犯，知识困在单个 Agent 里无法共享。

## 三个进化机制

| 进化类型 | 触发条件 | 效果 |
|---|---|---|
| AUTO-FIX | Skill 执行失败 | 自动修复指令，生成新版本 |
| AUTO-IMPROVE | Skill 执行成功但有优化空间 | 创建增强版本 |
| CAPTURE | 发现新的成功模式 | 从实际执行中提取为新 Skill |

**Benchmark**：50 专业任务，收入提升 **4.2 倍**，Token 降低 **46%**（GDPVal 评测，6 行业）

## 与 OpenClaw 关系

OpenClaw 的可选扩展，通过 MCP 协议接入，增加"执行层 + 自进化"能力。

## 发现的问题

### 问题一：不会自动被调用

目前 OpenClaw 接到任务不会自动触发 OpenSpace，必须显式说"交给 OpenSpace 执行"。

### 问题二：强模型 vs 多 system message 矛盾

| 模型 | 能力 | 多 system message |
|---|---|---|
| MiniMax M2.7 | 强 | ❌ 不支持 |
| Qwen 3.5 量化 | 弱 | ✅ 支持 |
| DeepSeek V3 | 强 | ✅ 支持 |

用强模型 → OpenSpace 执行失败；用支持多 system message 的模型 → 能力弱，Skill 质量存疑。

## 适用场景

适合：高频重复任务、工具链复杂任务、多 Agent 协作
不适合：一次性简单任务、专业领域高确定性要求

## 前提条件

1. LLM 支持多 system message
2. 任务有重复性

两个都满足 → 4.2x 收入提升；不满足 → 增加复杂度无收益
