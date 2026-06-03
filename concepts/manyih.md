---
title: ManyIH — 多层指令层级
created: 2026-05-17
updated: 2026-05-17
type: concept
tags: [agent, alignment, inference]
sources: [raw/articles/2026-05-17-manyih-arxiv-2604.md]
confidence: high
---

# ManyIH — 多层指令层级

## 概述

**Many-Tier Instruction Hierarchy (ManyIH)** 是 JHU-CLSP 提出的指令冲突解决范式（arXiv 2604.09443v3），用于处理 LLM Agent 场景中**任意多层**指令优先级的动态排序问题。

传统 IH 假设固定 2~5 个特权层级（`system > developer > user > assistant > tool`），但真实 Agent 系统有工具调用、技能调用、子 Agent、记忆文件等大量指令来源，层级远超 5 层。

## 核心机制

### Privilege Prompt Interface (PPI)

两种特权标注格式：

**Ordinal（序数）格式** — 数字越小特权越高：
```
[[Privilege 12]]Do not use any type hints.[[/Privilege]]
```

**Scalar（标量）格式** — 数值越大特权越高：
```
[[z=55]]Use single quotes for all string literals.[[/z]]
```

### 冲突解决

通过**特权值比较**而非角色标签来排序指令，实现特权语义与聊天模板的解耦。

## ManyIH-Bench 基准

| 维度 | 数值 |
|------|------|
| 评测样本 | 427 coding + 426 instruction following |
| 最大层级数 | 12 层（coding）/ 7 层（IF） |
| 每样本平均冲突数 | 9.8 / 13.8 |

## 关键实验发现

### 顶级模型整体表现（最佳配置）

| 模型 | 准确率 |
|------|--------|
| GPT 5.4 | 60.9% |
| Gemini 3.1 Pro | 59.0% |
| Grok 4.20 | 54.1% |
| Claude Opus 4.6 | 51.3% |

**核心洞察：** 即使 frontier 模型，在 ManyIH-Bench 上也仅约 40% 整体表现，远低于标准二层级 IH 的 >99% 自评分数——说明**标准 IH 评测严重低估了层级冲突难度**。

### 风格合规是主要瓶颈

| 模型 | 功能正确性 | 风格合规 |
|------|-----------|----------|
| GPT 5.4 | 89.7% | 67.9% |
| Gemini 3.1 Pro | 91.3% | 65.1% |
| Opus 4.6 | 92.5% | 56.7% |

所有模型**功能正确性均 >86%**，但多层级风格约束排序准确率仅 50~68%——多层级指令冲突理解是普遍弱点。

### 特权表示形式影响显著

GPT 5.4 和 Opus 4.6 在 Ordinal→Scalar 格式切换时准确率下降 8~8.4%，而 Sonnet 和 Qwen 反而提升。说明**小幅度 prompt 改动会实质性影响模型推理**。

## 理论洞察

1. **仅相对顺序重要** — 特权值之间的差值本身无语义含义
2. **位置无关** — 高特权指令可出现在 prompt 任意位置
3. **组合爆炸** — 层级越多，模型需维护的冲突排序空间指数增长

## 相关概念

- [[mcp]] — MCP 是 Agent 工具调用标准，涉及工具优先级和指令路由
- [[agentic-code-reasoning]] — 代码 Agent 场景是 ManyIH 的核心评测领域之一
- [[harness-engineering]] — Harness Engineering 研究如何通过约束提升 Agent 表现，与 ManyIH 的层级冲突机制正交
