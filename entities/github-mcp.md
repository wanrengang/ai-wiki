---
title: GitHub MCP
created: 2026-03-08
updated: 2026-03-08
type: entity
tags: [mcp, github, integration, tool, openclaw]
sources: [raw/articles/github mcp.md]
confidence: medium
---

# GitHub MCP

> GitHub MCP 服务器，为 AI 助手提供 GitHub 操作能力

## 概述

GitHub MCP 是 Model Context Protocol (MCP) 的一种实现，允许 AI 助手通过标准化的接口操作 GitHub 仓库。

## 功能

### 典型能力

| 功能 | 说明 |
|------|------|
| 仓库操作 | 创建、克隆、fork 仓库 |
| 文件操作 | 读取、创建、更新文件 |
| Issue 管理 | 创建、编辑、关闭 Issue |
| PR 管理 | 创建、审查、合并 Pull Request |
| 评论操作 | 提交评论、管理讨论 |

### 技术特点

- 基于 MCP 协议的标准接口
- 支持 OAuth 认证
- 与 OpenClaw 等 AI 平台集成

## 使用场景

### 开发者工作流

```
用户指令 → AI 助手 → GitHub MCP → GitHub API
```

### 典型应用

- **自动化代码审查**：AI 审查 PR 并评论
- **Issue 自动处理**：根据模板自动创建 Issue
- **文档自动更新**：代码变更时自动更新文档
- **工作流自动化**：自动合并符合条件的 PR

## 相关资源

- MCP 协议官网：https://modelcontextprotocol.io/
- GitHub MCP 源码：社区实现
