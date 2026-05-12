---
title: Agent Browser
created: 2026-03-03
updated: 2026-03-03
type: entity
tags: [browser, automation, AI-agent, Playwright, dev-tools]
sources: [raw/articles/2026-03-03-Agent-Browser-赋予AI真正的浏览器超能力.md]
---

# Agent Browser

## 概述

Agent Browser (dev-browser) 是一个革命性的浏览器自动化工具，专门为 AI Agent 设计。通过持久化的页面状态和基于 ref 的元素选择，让 AI 能够以增量式、可恢复的方式完成复杂的浏览器任务。

核心理念：**"Give your coding agent browser superpowers"**

## 核心架构

采用**客户端-守护进程（Client-Daemon）**架构：

```
graph TB
    A[AI Agent] -->|命令| B[Rust CLI]
    B -->|通信| C[Node.js Daemon]
    C -->|管理| D[Playwright Browser]
    D -->|持久化| E[页面状态]
    E -->|恢复| C
    C -->|返回结果| B
    B -->|紧凑输出| A
```

**三层设计**：
1. **Rust CLI 层**：解析命令，提供即时响应
2. **Node.js Daemon 层**：管理 Playwright 浏览器实例
3. **Browser 层**：执行实际的页面操作

## Ref 系统

传统的浏览器自动化依赖 CSS 选择器或 XPath，容易因 DOM 变化而失效。Agent Browser 引入了 **Ref (引用) 系统**：

1. **ARIA Snapshot**：获取页面的可访问性树
2. **自动编号**：为每个元素分配唯一 ref（如 `@e1`, `@e2`）
3. **精确定位**：通过 ref 直接与元素交互

**优势对比**：

| 特性 | 传统选择器 | Ref 系统 |
|-----|----------|---------|
| DOM 变化容忍度 | 低 | 高 |
| AI 友好性 | 需解析 JSON | 自然语言 |
| 上下文效率 | 3000-5000 tokens | 200-400 tokens |

## 页面状态持久化

脚本执行完毕后，浏览器**不关闭**，页面状态保持。

```typescript
// 脚本 1：打开页面并登录
const page = await client.page("github");
await page.goto("https://github.com/login");
// ...
await client.disconnect(); // ⚠️ 页面仍然保持打开状态

// 脚本 2：继续操作（登录状态保持）
const client = await connect();
const page = await client.page("github"); // 恢复已存在的页面
```

## 安装

```bash
# 全局安装
npm install -g agent-browser

# macOS
brew install agent-browser

# 即用即试
npx agent-browser open example.com
```

## 两种运行模式

### Standalone 模式
启动新的 Chromium 实例，适合本地开发测试。

### Extension 模式
连接到用户现有的 Chrome 浏览器，使用已有的登录凭证。

## 核心操作

```bash
# 打开网页
agent-browser open https://example.com

# 获取快照
agent-browser snapshot -i

# 点击元素
agent-browser click @e2

# 填写表单
agent-browser fill @e4 "hello@example.com"

# 截图
agent-browser screenshot page.png
```

## 与传统工具对比

| 特性 | Selenium | Playwright | Agent Browser |
|-----|---------|-----------|---------------|
| AI 友好性 | 低 | 中 | 高 |
| 上下文效率 | 高 DOM 开销 | 高 DOM 开销 | 紧凑输出 |
| 页面状态持久化 | 无 | 需手动实现 | 内置 |
| 失败恢复 | 从头开始 | 从头开始 | 断点续传 |

## 相关概念

- [[Chrome-DevTools-Protocol]]
- [[Browser-Automation]]
- [[Playwright]]
