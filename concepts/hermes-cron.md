---
title: Hermes Cron 定时任务
created: 2026-05-09
updated: 2026-05-09
type: concept
tags: [hermes, agent, automation, cron, tool]
sources: [raw/articles/2026-05-09-hermes-agent-cron.md]
confidence: high
---

# Hermes Cron 定时任务

## 概述

Hermes Agent 的 Cron 功能用于创建**定时自动化任务**：描述一次，AI 自动跑，结果发到指定位置，人不用在场。

最适合的场景：**固定频率 + 步骤明确 + 结果有格式要求**的重复性工作。

## 核心能力

### 三种创建方式

| 方式 | 说明 |
|------|------|
| 对话描述 | 直接说需求，Hermes 自动建任务 |
| `/cron` 命令 | 支持标准 Cron 表达式 |
| CLI | `hermes cron list/pause/resume/edit/run` |

### 四种时间格式

| 写法 | 含义 |
|------|------|
| `30m` / `2h` | 一次性延迟后执行 |
| `every 2h` | 固定间隔循环 |
| `0 9 * * *` | 标准 Cron，每天 9:00 |
| `every sunday 9am` | 自然语言 |

### 任务流水线（核心）

多个任务用 `context_from` 串联，前一个输出自动传入下一个：
- Job1 07:00 采集 → Job2 07:30 筛选 → Job3 08:00 推送

### 监控专用模式

- **[SILENT] 静默模式**：AI 回复以 `[SILENT]` 开头则不推送，静默保存日志
- **no-agent 脚本模式**：跳过 LLM，零 token 消耗，适合内存/磁盘监控

## 推送目标

可精确指定：`telegram`、`feishu`、`weixin`、`all`（广播所有平台）

## 注意事项

- 提示词必须自包含（新会话无上下文）
- 用 `--workdir` 指定工作目录，自动加载项目配置
- 任务不会套娃（防止无限循环）
- Gateway 需常驻运行

## 相关链接

- [[hermes-agent]] — 母体项目
- [[agent-engineering]] — Agent 工程化相关
- [[openclaw]] — 类似 Agent 编排框架
