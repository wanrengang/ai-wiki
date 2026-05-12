---
title: 钉钉 dingtalk-workspace-cli
created: 2026-04-09
updated: 2026-04-09
type: entity
tags: [dingtalk, cli, enterprise, ai-agent, golang]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-09-CLI工具-飞书钉钉赫尔墨斯.md]
---

# 钉钉 dingtalk-workspace-cli

## 概述

钉钉官方跨平台 CLI 工具，将钉钉全量产品能力统一封装，同时面向人类用户和 AI Agent 场景设计。

## 关键数据

| 指标 | 数据 |
|------|------|
| GitHub | DingTalk-Real-AI/dingtalk-workspace-cli |
| Stars | 1,535 |
| Forks | 79 |
| 语言 | Go |
| License | Apache 2.0 |

## 核心能力

| 能力 | 说明 |
|------|------|
| 消息发送/接收 | 群消息、个人消息 |
| 群管理 | 创建群、获取群信息、成员管理 |
| 机器人 | 接入 AI 能力 |
| 审批 | 审批流程操作 |
| 日历 | 日程管理 |
| 文档 | 钉钉文档操作 |

## 与其他 CLI 工具对比

| CLI 工具 | 官方 | 消息读取 | Stars | 语言 |
|---------|-----|---------|-------|-----|
| **Hermes Agent** | Nous Research | ❌ | 41,845 | Python |
| **dingtalk-workspace-cli** | 钉钉官方 | ✅ 需权限 | 1,535 | Go |
| **飞书** | 字节跳动 | ⚠️ 需 messageId | - | - |

## 消息读取 API

```bash
# 获取群消息历史
GET https://api.dingtalk.com/v1.0/im/messages

# 参数：
# - conversationId: 群ID
# - cursor: 分页游标
# - limit: 每页数量
# - startTime / endTime: 时间范围
```

## 企业场景选择建议

| 场景 | 推荐工具 |
|-----|---------|
| 企业内部（飞书） | OpenClaw |
| 企业内部（钉钉） | dingtalk-workspace-cli |
| 个人/研究用途 | Hermes Agent |
| 需要消息读取 | 钉钉 > 飞书 |

## 相关链接

- [[hermes-agent]] - Hermes Agent CLI
- [[dingtalk-wukong]] - 钉钉悟空
- [[feishu-lark-hermes]] - 飞书钉钉赫尔墨斯对比

## 标签

#CLI #钉钉 #dingtalk-workspace-cli #AI工具 #企业办公
