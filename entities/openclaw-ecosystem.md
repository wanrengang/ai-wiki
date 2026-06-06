---
title: OpenClaw 生态系统
created: 2026-03-08
updated: 2026-05-03
type: entity
tags: [openclaw, agent, ecosystem, open-source, china]
sources: [raw/articles/创业者围绕OpenClaw生态做什么产品.md, raw/articles/大厂突然长出一堆Claw打工人.md, raw/articles/Awesome-OpenClaw-Skills项目深度研究报告.md]
confidence: high
---

# OpenClaw 生态系统

> OpenClaw 是一个开源的多通道 AI Gateway，让 AI Agent 通过自然语言控制 Discord、Slack、Telegram、微信、飞书等平台。

## 概述

OpenClaw 是由 Peter Steinberger 创立的 AI Agent 平台，GitHub Stars 超过 111k。作为开源底座，它被国内大厂广泛套壳：QClaw、KimiClaw、ArkClaw、JVSClaw、AutoClaw 等"Claw"产品涌现，被称为"ACI (Artificial Claw Intelligence)"。

## 关键事实

| 属性 | 值 |
|------|-----|
| 创始人 | Peter Steinberger |
| 开源协议 | MIT |
| 技术栈 | Node.js, TypeScript |
| 支持渠道 | Discord, Slack, Telegram, 微信, 飞书, WhatsApp |
| 支持模型 | OpenAI, Anthropic, Gemini, Ollama |
| GitHub Stars | 111k+ |

## 生态产品

### 创业者产品（美国市场）

| 产品 | 月收入 | 定位 |
|------|--------|------|
| Claw Mart | $71k | OpenClaw 应用商店，卖配置模板 |
| OpenClaw Pro | $53k | 一键部署到云服务器 |
| RoofClaw | $50k | 预装 MacBook，卖给屋顶维修公司 |
| Donely | $44k | 云端部署，BYOK 模式 |
| SetupClaw | $33k | 白手套企业部署服务 |
| AI Money Group | $22k | 培训+付费社区 |
| StartClaw | $12k | "AI 员工"云端服务 |

### 中国大厂 Claw 产品

- **QClaw** — 月之暗面（Kimi）推出，3000亿港币估值
- **KimiClaw** — Kimi 衍生产品
- **ArkClaw** — Ark 证券
- **JVSClaw** — 致远互联
- **AutoClaw** — 自动化套件

## 核心架构

```
OpenClaw
├─ Node.js/TypeScript 核心
├─ 多渠道适配器 (Discord/Slack/Telegram/微信/飞书)
├─ 多模型路由 (OpenAI/Anthropic/Gemini/Ollama)
├─ 技能系统 (Skills) — Markdown 定义的工作流
├─ MCP 协议支持
└─ 记忆系统 (MEMORY.md + memory/ 目录)
```

## 版本更新（2026.3.7 → 2026.3.8）

**核心新特性：**
- Context Engine 插件接口 — 可替换上下文压缩策略
- ACP 持久化通道绑定 — Discord/Telegram 线程重启后保持
- Telegram 主题绑定增强
- Docker 多阶段构建 + slim 变体
- SecretRef 支持
- 30 个技能分类，5,494 个精选技能

**破坏性变更：** Gateway 认证模式要求显式设置 `gateway.auth.mode`

## 记忆体系（多飞书机器人）

支持三种模式：
1. **统一记忆** — 所有机器人共享同一记忆库
2. **独立记忆** — 每个机器人独立 workspace
3. **混合记忆（推荐）** — 公共知识共享 + 私有记忆隔离

## 与 Claude Dispatch 的竞争

Anthropic 在 30 天内连发四功能对标 OpenClaw：
| OpenClaw 能力 | Claude 回应 |
|---------------|-------------|
| WhatsApp 消息代理 | Dispatch：手机到桌面持久线程 |
| Discord/Telegram 控制 | Claude Code Channels：MCP 桥接 |
| 操作系统/浏览器控制 | Computer Use + Claude Code |
| 24小时守护进程 | 无（要求用户在场） |

**核心差异：** OpenClaw 追求"无摩擦"，Claude 追求"有护栏的安全"。

## 安全问题

- **CVE-2026-25253**：严重性 8.8/10，任何网站可静默接管 OpenClaw Agent
- **12% 的技能被确认恶意软件**：伪装成交易机器人窃取凭证

## 相关链接

- GitHub: https://github.com/openclaw/openclaw
- 文档: https://docs.openclaw.ai
- ClawHub: https://clawhub.com
- awesome-openclaw-skills: https://github.com/VoltAgent/awesome-openclaw-skills


## 相关概念

- [[openclaw]] — 生态系统核心项目
- [[hermes-agent]] — 生态内另一重要 Agent
- [[ai-agent]] — AI Agent 通用概念
