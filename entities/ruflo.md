---
title: Ruflo
created: 2026-05-11
updated: 2026-05-11
type: entity
tags: [agent-orchestration, multi-agent, claude-code, ruvnet, mcp, swarm, self-learning]
sources: [https://github.com/ruvnet/ruflo, my-obsidian/02AI与技术/2026-05-11-Ruflo-Agent编排平台深度研究.md]
confidence: high
---

# Ruflo

## 概述

Ruflo (前身为 Claude Flow) 是面向 Claude Code 的领先 Agent 编排平台，GitHub Stars 48,865。

**定位**：Claude Code 的"神经系统" — 自动路由任务、跨会话学习、协调多Agent后台运行。

## 关键数据

| 指标 | 数据 |
|------|------|
| GitHub | ruvnet/ruflo |
| Stars | 48,865 |
| Forks | 5,421 |
| License | MIT |
| 语言 | TypeScript |
| 创建时间 | 2025-06-02 |

## 核心架构

```
User --> Claude Code / CLI
         |
         v
  Orchestration Layer
  (MCP Server, Router, 27 Hooks)
         |
         v
  Swarm Coordination
  (Queen, Topology, Consensus)
         |
         v
  100+ Specialized Agents
  (coder, tester, reviewer, architect, security...)
         |
         v
  Memory & Learning
  (AgentDB, HNSW, SONA, ReasoningBank)
         |
         v
  LLM Providers
  (Claude, GPT, Gemini, Cohere, Ollama)
```

## 核心能力

| 能力 | 说明 |
|------|------|
| 🤖 100+ Agents | 专职Agent（编码/测试/安全/文档/架构） |
| 📡 Federated Comms | 零信任跨机器协作 |
| 🐝 Swarm Coordination | 层级/网格/自适应拓扑 |
| 🧠 Self-Learning | SONA + ReasoningBank 轨迹学习 |
| 💾 Vector Memory | HNSW 索引，150x-12,500x 加速 |
| ⚡ 12 Workers | 自动触发后台任务 |
| 🔌 Multi-Provider | Claude/GPT/Gemini/Cohere/Ollama |
| 🛡️ Security | AES-256-GCM + AIDefence |

## 两种安装路径

| 路径 | 适用场景 | 文件污染 | MCP注册 |
|------|---------|---------|---------|
| **Plugin** | 体验单个插件 | 零 | ❌ |
| **CLI 完整安装** | 生产环境 | .claude-flow/ | ✅ |

## 32 个原生插件

### 核心 (6)
ruflo-core, ruflo-swarm, ruflo-autopilot, ruflo-loop-workers, ruflo-workflows, ruflo-federation

### 记忆/RAG (5)
ruflo-agentdb, ruflo-rag-memory, ruflo-rvf, ruflo-ruvector, ruflo-knowledge-graph

### 智能/学习 (4)
ruflo-intelligence, ruflo-daa, ruflo-ruvllm, ruflo-goals

### 开发工具 (4)
ruflo-testgen, ruflo-browser, ruflo-jujutsu, ruflo-docs

### 安全 (2)
ruflo-security-audit, ruflo-aidefence

### 企业级 (11)
ruflo-adr, ruflo-ddd, ruflo-sparc, ruflo-migrations, ruflo-observability, ruflo-cost-tracker, ruflo-wasm, ruflo-plugin-creator, ruflo-iot-cognitum, ruflo-neural-trader, ruflo-market-data

## Web UI

| 地址 | 说明 |
|------|------|
| https://flo.ruv.io/ | 多模型聊天 + MCP tools |
| https://goal.ruv.io/ | GOAP A* 任务规划 |

## 与 OpenClaw 对比

| 维度 | Ruflo | OpenClaw |
|------|-------|----------|
| 定位 | Claude Code 扩展 | 通用Agent平台 |
| 核心能力 | Swarm + Self-Learning | Skills + 消息渠道 |
| 记忆 | HNSW向量库 | 向量+全文 |
| 联邦协作 | ✅ 零信任 | ⚠️ 需集成 |
| 消息渠道 | CLI/Telegram/Discord | 飞书/微信/Telegram |
| 定时任务 | 12 Workers | Cron |

## 安装

```bash
# 快速安装
curl -fsSL https://cdn.jsdelivr.net/gh/ruvnet/ruflo@main/scripts/install.sh | bash

# MCP server 模式
claude mcp add ruflo -- npx ruflo@latest mcp start
```

## 相关链接

- GitHub: https://github.com/ruvnet/ruflo
- Web UI: https://flo.ruv.io/
- GOAP: https://goal.ruv.io/

## 标签

#Ruflo #AgentOrchestration #ClaudeCode #MultiAgent #SwarmIntelligence #MCP #RAG #FederatedAI #SelfLearning
