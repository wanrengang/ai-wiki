---
title: MAPLE — 子代理架构：记忆·学习·个性化三元分解
created: 2026-05-18
updated: 2026-05-18
type: concept
tags: [agent, memory, architecture, personalization, learning]
sources: [raw/articles/2026-05-18-maple-sub-agent-memory-learning-personalization.md]
confidence: medium
---

# MAPLE — 子代理架构：记忆·学习·个性化三元分解

## 概述

MAPLE（Memory-Adaptive Personalized LEarning）来自 arXiv 2602.13258（ALA 2026 @ AAMAS 2026），提出一个核心洞察：**当前 AI 系统错误地将三个本质不同的机制视为统一整体**——Memory（记忆）、Learning（学习）、Personalization（个性化）。MAPLE 将其拆分为三个独立子代理，各自专用 LLM 实例，各自优化。

> **核心公式：** `Memory (ℳ) → retrieves → Personalization (𝒫) → adapts → response → feedback → Learning (ℒ) → writes → Memory`

## 三元分解

| 能力 | 回答什么问题 | 缺失时的失败模式 |
|------|------------|----------------|
| **Memory** | "关于这个用户，我们知道什么？" | 无法适应已知偏好 |
| **Learning** | "基于发生的事，我们应该知道什么？" | 永远重复错误 |
| **Personalization** | "基于我们所知，应该如何不同地响应？" | 优化平均用户（不存在） |

## 运行示例：Sarah vs. Marcus

同一问题"什么是 AI 中的 Transformer"：
- **Sarah**（资深 ML 工程师）：历史反馈讨厌过度简化 → 收到 PyTorch 多头注意力实现
- **Marcus**（产品经理，AI 新手）：持续使用类比 → 收到团队协作解读文档的类比

## 记忆三层透镜

**Lens 1 — 按形态：**
- Token 级（ℳₜ）：文本、记录、知识图谱节点（✅透明、可编辑）
- 参数化（ℳθ）：模型权重/LoRA 适配器（✅无缝；❌不透明）
- 潜变量（ℳ_z）：隐藏状态、KV cache（✅高效；❌短暂）

**Lens 2 — 按存储结构：**
- 平面（1D）：无序集合（向量数据库）
- 平面（2D）：显式关系（知识图谱）
- 分层（3D）：多层抽象（原始 → 摘要 → 用户模型 → 群体模式）

**Lens 3 — 认知类比：**
- 工作记忆：当前会话上下文
- 情景记忆：带时间锚定的特定事件
- 语义记忆：抽象事实去情境化知识
- 程序记忆：技能和习得行为

## 学习三层

| 层级 | 描述 | 示例 |
|------|------|------|
| **1: Replay** | 存储成功轨迹，按相似度检索 | Sarah 的 transformer 问题有效；模仿用于 attention |
| **2: Strategy Extraction** | 跨案例识别模式 | "调试时问架构的用户偏好理论与错误连接" |
| **3: Skill Synthesis** | 从积累经验中创建新能力 | 重复生成相似代码时合成工具函数 |

## 与 Mem0 的关键区别

| 方面 | Mem0 | MAPLE |
|------|------|-------|
| **范围** | 仅记忆 | 记忆 + 学习 + 个性化 |
| **学习** | 隐式（融合在检索中） | 显式符号提取 |
| **个性化** | 非一级关注 | 核心架构原语 |
| **架构** | 单体 | 三个独立专业子代理 |

## 相关概念

- [[mem0]] — AI Agent 通用记忆层，MAPLE 的直接对比对象
- [[decoction-experience-memory]] — 经验提精，中等记忆规模最优
- [[agent-drift-multi-agent-systems]] — 多智能体漂移问题，记忆退化是核心诱因
- [[ai-agent]] — AI Agent 技术全景
- [[harness-engineering]] — Agent 工程化，约束驱动范式
