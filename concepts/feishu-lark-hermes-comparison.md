---
title: 飞书钉钉赫尔墨斯对比
created: 2026-04-09
updated: 2026-04-09
type: concept
tags: [feishu, lark, dingtalk, hermes, cli, enterprise, comparison]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-09-CLI工具-飞书钉钉赫尔墨斯.md]
---

# 飞书钉钉赫尔墨斯对比

## 概述

三大 CLI 工具对比：OpenClaw（飞书）、dingtalk-workspace-cli（钉钉）、Hermes Agent（通用）。

## 主流 CLI 工具对比

| CLI 工具 | 官方 | 消息读取 | Stars | 语言 |
|---------|-----|---------|-------|-----|
| Hermes Agent | Nous Research | ❌ | 41,845 | Python |
| dingtalk-workspace-cli | 钉钉官方 | ✅ 需权限 | 1,535 | Go |
| 飞书 | 字节跳动 | ⚠️ 需 messageId | - | - |

## 功能丰富度对比

| 工具 | 消息 | 群管理 | 审批 | 日历 | 文档 |
|-----|-----|-------|-----|-----|-----|
| OpenClaw | ✅ | ⚠️ | ❌ | ❌ | ✅ |
| dingtalk-workspace-cli | ✅ | ✅ | ✅ | ✅ | ✅ |
| Hermes Agent | ⚠️ 仅私聊 | ❌ | ❌ | ❌ | ❌ |

## 企业场景选择

| 场景 | 推荐工具 |
|-----|---------|
| 企业内部（飞书） | OpenClaw |
| 企业内部（钉钉） | dingtalk-workspace-cli |
| 个人/研究用途 | Hermes Agent |
| 需要消息读取 | 钉钉 > 飞书 |

## 飞书 CLI 能力

### 当前能力
- ✅ 发送消息（单聊/群聊）
- ✅ 读取指定 messageId 的消息内容
- ❌ 不能直接浏览群历史记录

### API 读取群消息
```bash
GET https://open.feishu.cn/open-apis/im/v1/messages
```

## 钉钉 CLI 核心能力

| 能力 | 说明 |
|------|------|
| 消息发送/接收 | 群消息、个人消息 |
| 群管理 | 创建群、获取群信息、成员管理 |
| 机器人 | 接入 AI 能力 |
| 审批 | 审批流程操作 |
| 日历 | 日程管理 |
| 文档 | 钉钉文档操作 |

## Hermes Agent 特点

- ✅ 内置自我进化能力（自动创建/改进 Skills）
- ✅ 跨会话记忆 + FTS5 搜索
- ✅ 多种运行后端（Local/Docker/SSH/Daytona/Modal）
- ✅ OpenClaw 一键迁移
- ❌ 不支持飞书/钉钉

## 消息读取能力对比

| 平台 | CLI 读取群消息 | 限制 |
|-----|---------------|-----|
| 钉钉 | ✅ 支持 | 需要权限配置 |
| 飞书 | ⚠️ 需 messageId | 不能遍历历史 |

## 相关链接

- [[hermes-agent]] - Hermes Agent
- [[dingtalk-workspace-cli]] - 钉钉 CLI
- [[dingtalk-wukong]] - 钉钉悟空

## 标签

#CLI #钉钉 #飞书 #HermesAgent #AI工具 #企业办公
