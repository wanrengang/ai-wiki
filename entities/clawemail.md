---
title: ClawEmail
created: 2026-03-08
updated: 2026-03-08
type: entity
tags: [openclaw, skill, email, google-workspace, productivity]
sources: [raw/articles/ClawEmail深度解析-2026-03-08.md]
confidence: high
---

# ClawEmail

> OpenClaw 生态中的 Google Workspace 全功能集成技能，让 AI 助手直接操作 Gmail、Google Drive、Docs、Sheets、Slides。

## 概述

ClawEmail 是 OpenClaw 的核心技能之一，实现了对 Google Workspace 全套产品的 AI 控制。用户可以通过自然语言指令完成邮件处理、文档协作、数据分析等工作流。

## 核心功能

### Gmail 管理
- 读取、撰写、回复、转发邮件
- 搜索邮件、标记星标/重要
- 归档、删除、管理标签

### Google Drive 管理
- 上传/下载文件、搜索文件
- 创建文件夹、分享链接
- 移动/删除文件

### Google Docs 管理
- 创建/编辑文档、读取内容
- 添加评论、协作编辑
- 导出 PDF/Word

### Google Sheets 管理
- 创建电子表格、读取/写入数据
- 公式计算、数据分析
- 导出数据

### Google Slides 管理
- 创建演示文稿、添加幻灯片
- 编辑内容、插入图片
- 展示分享

## 技术架构

```
OpenClaw (AI Agent)
    ↓
ClawEmail Skill
    ↓
Google Workspace APIs
    ├─ Gmail API
    ├─ Drive API
    ├─ Docs API
    ├─ Sheets API
    └─ Slides API
```

**认证方式：** OAuth 2.0

## 适用场景

| 场景 | 操作 |
|------|------|
| 晨间邮件处理 | 标记重要、草拟回复、归档 newsletters |
| 项目文档协作 | 保存附件→创建 Docs→邀请协作→创建 Sheets 任务表 |
| 会议准备 | 搜索相关邮件→提取信息→创建 Docs 议程→生成 Slides→发送邀请 |

## 安装配置

```bash
# 通过 ClawHub 安装
npx clawhub@latest install clawemail

# 手动安装
git clone https://github.com/openclaw/skills.git
cp -r skills/cto1/clawemail ~/.openclaw/skills/
```

## 成本估算

- 轻度使用：$0-10/月
- 中度使用：$10-20/月
- 重度使用：$20-50/月

## 相关技能组合

- `clawemail + briefing` — 生成今日简报
- `clawemail + google-calendar` — 日程+邮件联动
- `clawemail + clickup-mcp` — 任务自动提取
