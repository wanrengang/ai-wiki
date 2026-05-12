---
title: nanobot
created: 2026-02-26
updated: 2026-02-26
type: entity
tags: [open-source, AI-agent, Python, lightweight]
sources: [raw/articles/2026-02-26-nanobot从入门到精通.md]
---

# nanobot

## 概述

nanobot 是由**香港大学数据智能实验室（HKUDS）**开发的超轻量级个人 AI 助手，于 2026 年 2 月 2 日正式发布。核心设计理念是"更小的代码，更强的功能"。

## 核心优势

| 特性 | nanobot | OpenClaw | 优势 |
|------|---------|----------|------|
| 代码行数 | ~4,000 行 | 430,000+ 行 | **小 99%** |
| 内存占用 | ~100MB | 数 GB | **更轻量** |
| 启动速度 | 秒级 | 分钟级 | **更快** |
| 学习曲线 | 平缓 | 陡峭 | **更易用** |
| 可读性 | 清晰易懂 | 复杂庞大 | **更易研究** |

## 核心特性

### 多平台集成

支持 10+ 主流聊天平台：

| 平台 | 协议 | 特点 |
|------|------|------|
| **Telegram** | Bot API | 推荐，简单易用 |
| **Discord** | Bot + Socket | 支持服务器和 DM |
| **WhatsApp** | Business API | 需要扫码认证 |
| **Feishu** | WebSocket | 企业办公友好 |
| **Slack** | Socket Mode | 无需公网 IP |
| **Email** | IMAP/SMTP | 邮件助手 |
| **QQ** | botpy SDK | 支持单聊 |
| **DingTalk** | Stream Mode | 钉钉集成 |

### 多 LLM 支持

支持 15+ 主流 LLM 提供商：

- **国际主流**：OpenAI、Anthropic Claude、Google Gemini
- **国内厂商**：DeepSeek、智谱 GLM、阿里 Qwen、月之暗面 Kimi
- **开源网关**：OpenRouter、AiHubMix、SiliconFlow
- **本地部署**：vLLM、LM Studio、llama.cpp

## 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                         nanobot 架构                          │
└─────────────────────────────────────────────────────────────┘

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  Chat Apps   │────────▶│   Gateway    │────────▶│    Agent     │
│              │  消息通道 │              │  核心代理 │              │
└──────────────┘         └──────────────┘         └──────────────┘
                               │                         │
                          ┌────┴────┐              ┌────┴────┐
                          │  Bus    │              │ Skills  │
                          │ 事件总线 │              │ 技能系统 │
                          └─────────┘              └─────────┘
```

## 核心模块

| 模块 | 功能 | 路径 |
|------|------|------|
| **Agent** | 核心代理循环 | `nanobot/agent/` |
| **Channels** | 消息通道管理 | `nanobot/channels/` |
| **Providers** | LLM 提供商注册 | `nanobot/providers/` |
| **Skills** | 技能系统 | `nanobot/skills/` |
| **Memory** | 记忆系统 | `workspace/memory/` |
| **Cron** | 定时任务 | `nanobot/cron/` |

## 安装方式

```bash
# 方法 1：从源码安装
git clone https://github.com/HKUDS/nanobot.git
cd nanobot && pip install -e .

# 方法 2：使用 uv 安装
uv tool install nanobot-ai

# 方法 3：从 PyPI 安装
pip install nanobot-ai
```

## 相关概念

- [[AI-Agent]]
- [[OpenCLAW]]
- [[Skills-System]]
- [[Multi-LLM]]
