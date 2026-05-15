---
title: OpenClaw Hooks 钩子系统
description: OpenClaw 的 Hooks 是一种事件驱动的自动化机制，可在特定事件（command:new、gateway:startup 等）发生时自动执行操作，支持 TypeScript 编写自定义钩子
type: concept
tags:
  - OpenClaw
  - Hooks
  - 钩子
  - 自动化
  - TypeScript
created: 2026-05-14
source: https://mp.weixin.qq.com/s/SL7QkpN6SlCUPc6PoZrX1g
author: 好玩AI应用
---

# OpenClaw Hooks 钩子系统

## 概述

Hooks 是 OpenClaw 的**事件驱动自动化机制**，当特定事件发生时自动执行预定义操作。

## 两种类型

### 内部 Hooks

运行在 Gateway 内部，特定事件触发：

| 事件 | 说明 |
|---|---|
| `command:new` | 输入 `/new` 时 |
| `command:reset` | 输入 `/reset` 时 |
| `gateway:startup` | OpenClaw 启动时 |
| `agent:bootstrap` | Agent 启动时 |

### Webhooks

对外 HTTP 接口，允许外部系统触发 OpenClaw 工作。

## 内置 4 个常用 Hook

| Emoji | 名称 | 作用 |
|---|---|---|
| 💾 | session-memory | 每次 `/new` 自动保存会话到 memory |
| 📎 | bootstrap-extra-files | 启动时注入额外 bootstrap 文件 |
| 📝 | command-logger | 记录所有命令到日志 |
| 🚀 | boot-md | 启动时运行 BOOT.md |

## 命令

```bash
openclaw hooks list       # 查看所有 Hooks
openclaw hooks enable <name>  # 启用 Hook
openclaw hooks check      # 查看状态
openclaw hooks info <name>   # 详细信息
```

## 自定义 Hook 结构

```
my-hook/
├── HOOK.md       # 元数据 + 说明
└── handler.ts    # TypeScript 处理逻辑
```

## Hooks vs Cron vs Heartbeat

| | Hooks | Cron | Heartbeat |
|---|---|---|---|
| 触发 | 事件 | 时间 | 定时+巡检 |
| 场景 | 响应操作 | 定时任务 | 周期性检查+提醒 |
| 难度 | 写代码 | 会命令 | 会配置 |

## 发现顺序（优先级递增）

内置 → 插件 → 托管（`~/.openclaw/hooks/`）→ 工作区（`workspace/hooks/`）

工作区钩子可新增但不可覆盖同名上层钩子。
