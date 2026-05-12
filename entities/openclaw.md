---
title: OpenCLAW
created: 2026-02-12
updated: 2026-02-12
type: entity
tags: [open-source, AI-agent, GitHub-Actions, platform]
sources: [raw/articles/2026-02-12-OpenCLAW近半月更新与全面技术报告.md]
---

# OpenCLAW

## 概述

OpenCLAW 是一个开源的个人 AI 助手项目，支持在任何操作系统和平台上运行。基于 GitHub Actions 的 AI 智能体平台，核心理念是让 AI 智能体成为数字工作伙伴。

## 版本信息

| 版本号 | 发布日期 | 主要特性 |
|--------|----------|----------|
| v2026.2.9 | 2026-02-09 | 最新版本，持续优化 |
| v2026.2.6 | 2026-02-06 | 重大功能更新 |
| v2026.1.30 | 2026-01-31 | CLI 体验优化，Kimi 免费执行 |
| v2026.1.29 | 2026-01-29 | 安全补丁版本 |

## 核心架构

```
┌─────────────────────────────────────────────────────────┐
│                     OpenCLAW 架构                        │
├─────────────────────────────────────────────────────────┤
│  用户界面层 (UI Layer)                                  │
│  • Web Chat  • CLI  • Desktop  • Mobile                │
│                         ↓                                 │
│  核心引擎层 (Core Engine)                               │
│  • 任务调度  • 记忆管理  • 工具调用  • 插件系统         │
│                         ↓                                 │
│  渠道适配层 (Channel Layer)                             │
│  • Telegram  • Slack  • Discord  • Twitch  • 更多...   │
│                         ↓                                 │
│  模型网关层 (Gateway Layer)                             │
│  • LLM 集成  • API 管理  • 负载均衡  • 缓存            │
└─────────────────────────────────────────────────────────┘
```

## 技术栈

| 层级 | 技术选型 | 用途 |
|------|----------|------|
| **前端** | TypeScript + React | 用户界面开发 |
| **后端** | Node.js + TypeScript | 服务端逻辑 |
| **数据库** | SQLite + 向量数据库 | 数据持久化 |
| **容器化** | Docker | 部署与分发 |
| **AI 框架** | Claude SDK | AI 能力集成 |

## 支持的模型提供商

| 提供商 | 模型类型 | 特点 |
|--------|----------|------|
| OpenAI | GPT-4 系列 | 通用能力强 |
| Anthropic | Claude 系列 | 安全可靠 |
| Google | Gemini 系列 | 多模态支持 |
| Moonshot | Kimi 系列 | 中文优化 |
| 智谱 AI | GLM 系列 | 中文理解 |
| MiniMax | 海螺系列 | 高效推理 |
| 小米 | MiMo 系列 | 端侧部署 |

## 安全更新

### CVE-2026-24763 漏洞修复
- **漏洞类型**：命令注入（Command Injection）
- **影响版本**：v2026.1.29 之前版本
- **修复措施**：输入验证加强、命令执行沙箱化、权限控制优化

## 相关概念

- [[AI-Agent]]
- [[OpenCode]]
- [[nanobot]]
- [[MCP]]
