---
title: OpenCode Platform
created: 2026-02-25
updated: 2026-02-25
type: entity
tags: [OpenCode, AI-agent, SDK, platform]
sources: [raw/articles/2026-02-25-基于OpenCode构建个人助手智能体平台.md]
---

# OpenCode Platform

## 概述

OpenCode 是一个强大的 Agent 编排平台，其核心理念是让 AI 智能体成为真正的数字工作伙伴。不仅是一个 AI 编程助手，更是一个完整的 Agent 编排平台。

## 核心架构

```
┌─────────────────────────────────────────────────────────────┐
│                    OpenCode Server                          │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    Agent 调度器                             │
└─────────────────────────────────────────────────────────────┘
          ↓                    ↓                    ↓
   ┌────────────┐       ┌────────────┐       ┌────────────┐
   │ Skills系统 │       │ Tools系统  │       │ MCP集成    │
   └────────────┘       └────────────┘       └────────────┘
```

## 技能系统（Skills System）

每个技能都是一个独立的、可复用的功能模块：

| 技能类型 | 用途 | 示例 |
|---------|------|------|
| **探索型** | 代码库搜索和模式发现 | explore、librarian |
| **执行型** | 特定任务的高效处理 | quick、git-master |
| **分析型** | 深度推理和架构设计 | oracle、metis |
| **协作型** | 多 Agent 并行协调 | build、test |

## 工具系统（Tools System）

**核心工具类别**：

1. **文件操作工具**
   - `read`：读取文件内容
   - `write`：写入文件
   - `edit`：编辑文件
   - `glob`：文件模式匹配

2. **代码分析工具**
   - `lsp_diagnostics`：语言服务器诊断
   - `lsp_goto_definition`：跳转到定义
   - `lsp_find_references`：查找引用
   - `ast_grep_search`：AST 模式搜索

3. **搜索工具**
   - `grep`：内容搜索
   - `glob`：文件搜索

4. **MCP 集成工具**
   - `skill_mcp`：调用 MCP 服务
   - 支持自定义 MCP 服务器

## 多 Agent 协作机制

**协作模式**：
- **并行执行**：多个 Agent 同时工作，互不阻塞
- **会话保持**：`session_id` 机制保持上下文连续性
- **结果聚合**：自动收集和整合各 Agent 的结果
- **任务取消**：支持单个或批量取消后台任务

## SDK API 核心接口

```typescript
// 创建会话
const session = await client.session.create({
  body: { title: `${agent.name} - ${context.taskId}` }
})

// 发送提示
await client.session.prompt({
  sessionID: sessionId,
  noReply: true,
  parts: [{ type: "text", text: systemPrompt }]
})

// 并行任务执行
const taskAgent = task({
  category: "deep",
  load_skills: [skill],
  prompt: skillPrompt,
  run_in_background: true
})
```

## 与 OpenClaw 的对比

| 能力维度 | OpenClaw | OpenCode | 可行性评估 |
|---------|----------|----------|-----------|
| **Agent 定义** | YAML 配置 | Skills + Category | ✅ 完全支持 |
| **工作流触发** | GitHub Actions | SDK + Webhook | ✅ 完全支持 |
| **工具系统** | 插件系统 | Tools + MCP | ✅ 完全支持 |
| **多 Agent 协作** | 支持 | 原生支持 | ✅ 优势更明显 |
| **代码库集成** | Git 操作 | 完整代码库操作 | ✅ 更强大 |

## 相关概念

- [[OpenCLAW]]
- [[AI-Agent]]
- [[Skills-System]]
- [[MCP]]
