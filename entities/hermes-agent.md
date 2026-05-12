---
title: Hermes Agent
created: 2026-04-09
updated: 2026-04-09
type: entity
tags: [nous-research, cli, agent, self-evolving, python, open-source]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-09-CLI工具-飞书钉钉赫尔墨斯.md]
---

# Hermes Agent

## 概述

"The agent that grows with you" — 来自 Nous Research 的自我进化 AI Agent。

## 关键数据

| 指标 | 数据 |
|------|------|
| GitHub | NousResearch/hermes-agent |
| Stars | 41,845 |
| Forks | 5,349 |
| 语言 | Python |
| License | MIT |
| 官网 | https://hermes-agent.nousresearch.com |

## 安装

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

## 基本命令

| 命令 | 说明 |
|-----|------|
| `hermes` | 启动交互式对话 |
| `hermes model` | 选择模型 |
| `hermes gateway` | 启动消息网关 |
| `hermes skills` | 查看技能 |
| `hermes claw migrate` | 从 OpenClaw 迁移 |

## 支持的消息平台

- CLI (终端)
- Telegram
- Discord
- Slack
- WhatsApp
- Signal
- Email

## 核心特点

| 特点 | 说明 |
|------|------|
| 自我进化 | 自动创建/改进 Skills |
| 跨会话记忆 | + FTS5 搜索 |
| 多种运行后端 | Local/Docker/SSH/Daytona/Modal |
| OpenClaw 迁移 | 一键迁移支持 |
| 飞书/钉钉 | ❌ 不支持 |

## 与钉钉 CLI 对比

| 工具 | 消息 | 群管理 | 审批 | 日历 | 文档 |
|-----|-----|-------|-----|-----|-----|
| OpenClaw | ✅ | ⚠️ | ❌ | ❌ | ✅ |
| dingtalk-workspace-cli | ✅ | ✅ | ✅ | ✅ | ✅ |
| Hermes Agent | ⚠️ 仅私聊 | ❌ | ❌ | ❌ | ❌ |

## 企业场景选择

| 场景 | 推荐工具 |
|-----|---------|
| 企业内部（钉钉） | dingtalk-workspace-cli |
| 个人/研究用途 | Hermes Agent |

## 相关链接

- [[dingtalk-workspace-cli]] - 钉钉 CLI
- [[feishu-lark-hermes]] - 飞书钉钉赫尔墨斯对比
- [[claude-managed-agents]] - Claude Managed Agents

## 标签

#HermesAgent #NousResearch #AI工具 #CLI #Agent
