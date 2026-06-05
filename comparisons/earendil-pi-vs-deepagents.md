---
title: pi vs Deep Agents
created: 2026-06-05
updated: 2026-06-05
type: comparison
tags: [comparison, harness, coding-agent, open-source]
sources: [raw/articles/2026-06-05-earendil-pi-agent-harness.md]
confidence: high
---

# pi vs Deep Agents

两个开源 AI Coding Agent Harness 的技术路线对比。^[raw/articles/2026-06-05-earendil-pi-agent-harness.md]

## 核心差异

| 维度 | [[earendil-pi]] | [[deepagents]] |
|------|-----------------|----------------|
| **主语言** | TypeScript（Node.js） | Python 优先 + JS/TS 版 |
| **底层依赖** | 自研 harness，无外部 AI 框架 | 强绑定 LangGraph / LangChain |
| **LLM 抽象层** | 自研 `pi-ai` | 复用 LangChain chat model 接口 |
| **Stars** | 59,845 | 23,900 |
| **创建时间** | 2025-08 | 2025-07 |
| **可观测性** | 无内置方案 | LangSmith 一站式（tracing/evaluation/deployment） |
| **权限系统** | 无内置（文档提供 3 种 sandbox 方案） | 无内置 |
| **生态锁定** | 无锁定 | 深度绑定 LangChain 生态 |

## 定位哲学

**pi — 干净的地基**
刻意不引入 LangChain 这类重型框架，自己实现 agent loop、tool calling、状态管理。好处是干净、可控、依赖链短；代价是企业接入时要自己搞定监控、部署、可复现性。

**Deep Agents — LangChain 生态里的 opinionated harness**
建在 LangGraph 上面，把 sub-agents、context management、skills、human-in-the-loop 全部打包，开箱即用。代价是绑死 LangChain 生态。

## 企业选型建议

| 场景 | 推荐 |
|------|------|
| 已在用 LangChain / LangSmith | [[deepagents]] 接入成本极低 |
| 不想被任何 AI 框架锁定 | [[earendil-pi]] 更干净 |
| 需要 Python + 生产可观测性 | [[deepagents]] LangSmith 集成现成 |
| 需要 multi-provider 切换 | [[earendil-pi]] 的 `pi-ai` 抽象层是亮点 |
| 团队 TypeScript 技术栈 | [[earendil-pi]] |
| 有 LangChain 定制需求 | [[deepagents]] |

## 共同点

- 开源 MIT License
- 支持任意支持 tool calling 的模型（frontier API / open-weight / local）
- 内置文件系统、Shell 访问工具
- 面向扩展设计，支持自定义工具和 sub-agents
- 非常活跃的维护（均在 2026-06 有近期更新）
- 均无内置权限系统，需企业自行加固

## 结论

两者在做同一件事：**把"能跑起来的 coding agent harness"做成开源产品**，但走了完全不同的技术路径——pi 是 TypeScript 自底向上搭，deep-agents 站在 Python/LangChain 生态肩膀上。这个差异本质上映射了 AI Agent 工程领域的两条路线：**轻量自主可控** vs **生态借力快速落地**。
