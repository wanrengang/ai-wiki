---
title: LangChain Deep Agents
created: 2026-06-04
updated: 2026-06-05
type: entity
tags: [open-source, coding-agent, harness, python, langchain, langgraph]
sources: [raw/articles/2026-06-05-earendil-pi-agent-harness.md]
confidence: high
---

# Deep Agents

## 概述

LangChain 官方开源 Agent Harness，"batteries-included"理念——把 sub-agents、文件系统、context 管理、shell 访问、持久记忆、human-in-the-loop、skills 系统全部打包，开箱即用。^[raw/articles/2026-06-05-earendil-pi-agent-harness.md]

与 LangChain 生态深度集成：底层用 LangGraph（streaming、persistence、checkpointing），上线后接入 LangSmith 做 tracing、evaluation 和监控部署。

## 关键事实

| 属性 | 值 |
|------|-----|
| **Stars** | 23,900（2026-06-05） |
| **Forks** | 3,377 |
| **语言** | Python 优先，TS/JS 版见 [deepagents.js](https://github.com/langchain-ai/deepagentsjs) |
| **创建时间** | 2025-07-27 |
| **License** | MIT |
| **PyPI 月下载** | 4,845,103 |
| **PyPI 周下载** | 1,149,899 |
| **PyPI 日下载** | 219,597 |
| **最新版本** | 0.6.8 |
| **Open Issues** | 126 |

## 核心依赖

- langchain, langchain-anthropic, langchain-core, langchain-google-genai, langsmith, wcmatch

## 技术定位

Deep Agents 位于 LangChain 生态的中间层：

```
LangGraph（底层图运行时）
  ↓
LangChain create_agent（轻量 harness）
  ↓
Deep Agents（opinionated harness，包揽 middleware）
```

三层均可组合：任何 LangGraph `CompiledStateGraph` 都能作为 sub-agent 注入 Deep Agents，自定义编排与默认能力并存。^[raw/articles/2026-05-16-memory-processing-pipeline-llm-inference.md]

## 核心功能（8 大能力）

- **Sub-agents** — 任务委托给独立上下文窗口的 agent，支持 async subagents
- **Filesystem** — 读写编辑搜索，支持可插拔的本地/sandbox/远程后端
- **Context management** — 长对话线程摘要化，tool 输出 offload 到磁盘
- **Shell access** — 在选定的 sandbox 中运行命令
- **Persistent memory** — 可插拔状态和存储后端，跨会话记忆
- **Human-in-the-loop** — 工具调用前审批、编辑、拒绝
- **Skills** — 按需加载的可复用行为模块
- **Tools** — 自定义函数或任意 MCP server

## Deep Agents Code（dcode）

内置预置 CLI——终端 coding agent，安装方式：

```bash
curl -LsSf https://langch.in/dcode | bash
```

与 Claude Code / Cursor 功能相近，支持：
- 任意 tool-calling LLM，session 中途切换 provider 或 model
- `/auth` 命令配置 API key（OpenAI/Anthropic/Google 开箱即用；Ollama/Groq/xAI 按需安装）
- Web 搜索（需 TAVILY_API_KEY）
- Persistent memory 跨会话携带上下文
- 自定义 Skills 塑造 agent 行为
- Approval controls 控制代码执行
- LangSmith tracing（可选）

> Deep Agents Code 暂不支持 Windows，建议 WSL 环境运行。

## 设计哲学

- **Opinionated defaults** — 长时域、多步骤工作的默认配置已调好，不用从零搭
- **Fully extensible** — 覆盖或替换任意组件无需 fork
- **Model-agnostic** — 任何支持 tool calling 的模型均可：frontier API / open-weight / local（Ollama / vLLM / llama.cpp）

## 安全模型

Deep Agents 采用"信任 LLM"模型——agent 能做任何工具允许的事。边界管控在工具/sandbox 层面强制，而非依赖模型自我约束。

## 定位对比

| 场景 | 推荐 |
|------|------|
| 想要完整 harness 开箱即用 | Deep Agents |
| 只需轻量 harness 不要 middleware | LangChain `create_agent` |
| agent loop 形态不匹配需要自定义图 | LangGraph |

## 相关概念

- [[earendil-pi]] — TypeScript 自研 harness，与 Deep Agents 形成生态路线对比
- [[ai-agent]] — AI Agent 通用概念
- [[harness-engineering]] — Harness 工程化
