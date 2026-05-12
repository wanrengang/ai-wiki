---
title: MCP (Model Context Protocol)
created: 2026-02-28
updated: 2026-02-28
type: concept
tags: [MCP, Protocol, AI, Agent, Tool-Calling]
sources: [raw/articles/2026-02-28-国内MCP服务器生态调研报告.md]
---

# MCP (Model Context Protocol)

## 概述

MCP（Model Context Protocol，模型上下文协议）是 Anthropic 于 2024 年 11 月正式发布的**开放标准协议**，旨在为 AI 大模型与外部数据源、工具之间建立统一的通信规范。

## 核心定位

MCP 的本质是**AI 时代的 USB 协议**——就像 USB Type-C 为设备连接提供统一接口，MCP 为 AI 模型与外部世界的交互提供标准化方式。

## 架构设计

```
┌─────────────┐         ┌─────────────────────┐         ┌─────────────┐
│   AI Model  │ ←─────→ │   MCP Host          │ ←─────→ │ MCP Servers │
│  (LLM/Claude)│         │ (Claude Desktop/IDE)│         │ (Filesystem)│
└─────────────┘         └─────────────────────┘         │  (GitHub)   │
                                                         │  (Slack)    │
                                                         │  (Database) │
                                                         └─────────────┘
```

## 核心原语

### 1. Resources（资源）
MCP 服务器暴露的只读数据资源：
- 文件内容
- 数据库记录
- API 响应
- 配置信息

### 2. Tools（工具）
AI 模型可以调用的函数：
```json
{
  "name": "filesystem_read_file",
  "description": "读取文件内容",
  "inputSchema": {
    "type": "object",
    "properties": {
      "path": { "type": "string" }
    }
  }
}
```

### 3. Prompts（提示模板）
预定义的提示词模板，可携带动态变量：
```json
{
  "name": "review_code",
  "description": "代码审查",
  "arguments": [
    { "name": "file", "type": "string" }
  ]
}
```

## 国内 MCP 生态

### 已支持的服务商

| 服务商 | 特点 | 支持工具数 |
|--------|------|-----------|
| **阿里云** | 百炼平台内置 | 50+ |
| **字节跳动** | 火山引擎整合 | 30+ |
| **微信 AI** | 微信生态深度集成 | 40+ |

### 典型 MCP 服务器

#### 官方/知名
- `filesystem`：本地文件系统访问
- `github`：GitHub API 操作
- `slack`：Slack 消息发送
- `postgres`：数据库查询
- `sqlite`：轻量级数据库

#### 国内特色
- `aliyun-ecs`：阿里云 ECS 管理
- `doubao-search`：字节搜索能力
- `wechat-work`：企业微信操作

## MCP vs 传统 Tool-Calling

| 维度 | MCP | 传统 Tool-Calling |
|------|-----|------------------|
| **标准化程度** | 统一协议 | 各家自定义 |
| **生态扩展** | 开放生态 | 封闭生态 |
| **开发成本** | 一次开发，多处运行 | 每平台单独适配 |
| **安全模型** | 标准化权限控制 | 平台自定义 |

## 发展趋势

1. **协议普及**：预计 2026 年底成为 AI 行业事实标准
2. **生态丰富**：MCP Servers 注册量持续增长
3. **工具标准化**：更多服务提供 MCP 接口
4. **安全增强**：标准化权限模型和审计机制

## 相关实体

- [[nanobot]]
- [[OpenCLAW]]
- [[dify]]

## 相关概念

- [[AI-Agent]]
- [[Tool-Calling]]
- [[Function-Calling]]
