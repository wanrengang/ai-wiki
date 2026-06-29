---
title: SkillWeaver — Compositional Skill Routing for LLM Agents
created: 2026-06-18
updated: 2026-06-18
type: concept
tags: [agent, mcp, routing, benchmark, training]
sources: [raw/papers/2026-06-18-skillweaver-compositional-skill-routing-2606.18051.md]
confidence: high
---

## 概述

**SkillWeaver**（阿里云，arXiv 2606.18051）是首个解决**多技能组合路由**问题的框架。核心洞察：**任务分解质量是瓶颈**，而非检索本身。

现实任务往往需要多个技能协作（如"下载数据→转换→生成报告"），但现有工作只研究单技能选择。SkillWeaver 正式提出 **Compositional Skill Routing** 问题：给定复杂 query 和大规模 skill 库，将其分解为原子子任务→检索对应 skill→组合为可执行计划。

## 核心方法

**三阶段框架 SkillWeaver：**

1. **Decompose（分解）：** LLM 任务分解器将复杂 query 拆解为原子子任务
2. **Retrieve（检索）：** Bi-encoder retriever + FAISS，在 2,209 个 MCP skill 中语义检索候选
3. **Compose（组合）：** 依赖感知的 DAG planner，基于 skill 间兼容性选择最终执行链

**关键创新：Iterative Skill-Aware Decomposition (SAD)**

检索增强的反馈循环——将可用 skill 库的元信息注入分解过程，迭代对齐分解粒度与 skill 词汇表。单次迭代即将 DA（分解准确率）从 51.0% 提升至 67.7%（+32.7%，p<10⁻⁶）。

## 基准测试：CompSkillBench

- **规模：** 300 个组合查询，覆盖 2,209 个真实 MCP server skills，24 个功能类别
- **难度分级：** Easy（2 skills）×150，Medium（3 skills）×100，Hard（4-5 skills）×50
- **核心指标：** DA（严格分解准确率）、CatR@k（类别召回@k）、Chain Exact Match

## 实验结果

| 配置 | DA | CatR@1 | CatR@10 |
|------|-----|--------|---------|
| Vanilla（Qwen2.5-7B） | 51.0% | 34.2% | 68.6% |
| +SAD（H=15） | 67.7% | 37.0% | 70.3% |

- **难度分析：** Easy DA 44.7%→63.3%（+41.6%），Medium 66.0%→78.0%（+18.2%），Hard 40.0%→60.0%（+50.0%）
- **LLM-Direct 天花板：** qwen-max 给 100 个 skill 名称，DA=90% 但 CatR@1 仅 21.1%——说明仅在 prompt 中列出 skill 不足以解决匹配问题
- **ReAct 对比：** DA=0%（think-act-observe 循环将多步任务压缩为单步）
- **泛化能力：** SAD 在类别 held-out（+35.6% relative DA gain）和随机 skill held-out（+23.2%）场景均保持优势

## 关键洞察

1. **分解是瓶颈，不是检索：** 正确粒度是有效检索的前提条件（DA=1 时 CatR@1 从 34% 升至 41%）
2. **元数据检索足够：** 仅用 skill 元数据编码即可达到 CatR@10=69.0%
3. **SAD 是通用解：** 不仅仅是 skill routing 的解，也是任何需要将任务边界与可用工具库对齐的场景的解
4. **Context 压缩 99%：** 相比直接让 LLM 看过长 skill 列表，SkillWeaver 大幅降低上下文消耗

## 相关概念

[[mcp]] — SkillWeaver 评测基于 MCP（Model Context Protocol）生态的 2,209 个真实 skill  
[[harness-engineering]] — PE→CE→HE 演进框架，约束驱动 Agent 行为优化  
[[enterprise-skill-architecture]] — 企业级 Skill 体系，SkillWeaver 的实用场景  
[[openclaw-skills-list]] — OpenClaw 生态的 Skills 榜单，Skill 路由的工程实践  
[[test-time-compute-scaling]] — 推理时计算 scaling，SAD 的迭代反馈机制与其有共通之处
