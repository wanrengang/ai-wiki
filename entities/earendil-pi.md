---
title: earendil-works/pi
created: 2026-06-05
updated: 2026-06-05
type: entity
tags: [open-source, coding-agent, harness, typescript, llm]
sources: [raw/articles/2026-06-05-earendil-pi-agent-harness.md]
confidence: high
---

# earendil-works/pi

## 概述

TypeScript 实现的 AI Coding Agent Harness，近 6 万 Star 的开源项目。作者是 badlogic（Mario Zechner），公司为 Earendil Works。^[raw/articles/2026-06-05-earendil-pi-agent-harness.md]

## 关键事实

| 属性 | 值 |
|------|-----|
| **Stars** | 59,845（2026-06-05） |
| **Forks** | 7,180 |
| **语言** | TypeScript |
| **创建时间** | 2025-08-09 |
| **License** | MIT |
| **Commits** | 4,427 |

## 核心包架构

| 包 | 职责 |
|----|------|
| **@earendil-works/pi-ai** | 统一多 LLM Provider 接口（OpenAI / Anthropic / Google / AWS Bedrock / Mistral） |
| **@earendil-works/pi-agent-core** | Agent 运行时内核，transport 抽象、状态管理、attachment 支持 |
| **@earendil-works/pi-coding-agent** | 交互式 CLI，内置 read / bash / edit / write 工具 + 会话管理 |
| **@earendil-works/pi-tui** | 自研终端 UI 库，differential rendering 高效渲染 |

## 设计决策

### 无内置权限系统
pi 默认以启动用户的完整权限运行 fs / process / network / credentials。作者明确表态：需要边界管控的用户自行容器化+sandbox。文档提供了三种模式：OpenShell（全流程 sandbox）、Gondolin extension（pi 留在 host，内置工具路由进 micro-VM）、Plain Docker。

### Supply-chain Hardening
- 直接依赖 pin 到 exact version
- `save-exact=true`，`min-release-age=2` 防止同日版本劫持
- `package-lock.json` 是 ground truth
- CLI 包含 shrinkwrap 锁死 transitive deps
- 发布前在 repo 外做隔离安装验证

### 开源 Session 数据共享
作者在推动用户把 coding agent 的工作 session 数据公开上传 Hugging Face，用的工具是 [badlogic/pi-share-hf](https://github.com/badlogic/pi-share-hf)。核心观点：真实开源项目的 coding session 比 toy benchmark 更能训练出真正有用的 coding agent。作者自己的数据集 [badlogicgames/pi-mono](https://huggingface.co/datasets/badlogicgames/pi-mono) 已公开。

## 企业价值

- 避免 LLM Provider 锁定，multi-provider 切换无需改上层逻辑
- 代码不出域，适合有数据主权要求的企业
- Extension 系统支持内部工具链集成（Jira、Confluence、内部平台）
- 作为 baseline 评估商业方案（GitHub Copilot Enterprise 等）的性价比

## 局限性

- 无内置权限系统，需企业自行加固
- 无内置可观测性方案（监控/ tracing / evaluation），需自搭
- 适合有工程能力的团队，不适合直接发给普通业务用户

## 相关概念

- [[deepagents]] — LangChain 官方 Agent Harness，Python 优先，与 pi 形成生态路线对比
- [[ai-agent]] — AI Agent 通用概念
- [[harness-engineering]] — Agent Harness 工程化
