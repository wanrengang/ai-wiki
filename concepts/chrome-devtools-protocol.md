---
title: Chrome DevTools Protocol
created: 2026-02-06
updated: 2026-02-06
type: concept
tags: [Chrome, DevTools, Protocol, CDP, Browser-Automation]
sources: [raw/articles/2026-02-06-Chrome-DevTools-Protocol技术调研报告.md]
---

# Chrome DevTools Protocol (CDP)

## 概述

Chrome DevTools Protocol（CDP）是 Chrome 浏览器提供的**远程调试协议**，允许开发者通过 WebSocket 连接直接与 Chrome 进行通信，实现浏览器自动化、性能监控、调试等能力。

## 架构原理

```
┌─────────────┐     WebSocket      ┌─────────────────────┐
│  DevTools   │ ←───────────────→  │  Chrome Browser     │
│   Client    │                    │  (Chromium Engine)  │
└─────────────┘                    └─────────────────────┘
      ↑                                    ↑
      │                                    │
      ↓                                    ↓
  Puppeteer/                          CDP Commands
  Playwright                          & Events
```

## 核心能力分类

### 1. 标签页管理

| 命令 | 功能 |
|------|------|
| `Target.createTarget` | 创建新标签页 |
| `Target.closeTarget` | 关闭标签页 |
| `Target.getTargets` | 获取所有目标 |
| `Target.activateTarget` | 激活目标 |

### 2. 页面操作

| 命令 | 功能 |
|------|------|
| `Page.navigate` | 导航到 URL |
| `Page.reload` | 刷新页面 |
| `Page.captureScreenshot` | 截图 |
| `Page.printToPDF` | 生成 PDF |

### 3. JavaScript 执行

| 命令 | 功能 |
|------|------|
| `Runtime.evaluate` | 执行 JS 代码 |
| `Runtime.callFunctionOn` | 在对象上调用函数 |
| `Debugger.evaluateOnCallFrame` | 在特定帧执行 |

### 4. 网络监控

| 事件 | 功能 |
|------|------|
| `Network.requestWillBeSent` | 请求即将发送 |
| `Network.responseReceived` | 收到响应 |
| `Network.dataReceived` | 数据接收中 |

### 5. 性能分析

| 命令 | 功能 |
|------|------|
| `Performance.enable` | 启用性能监控 |
| `Performance.getMetrics` | 获取性能指标 |

## 工具生态

### Puppeteer
Google 官方维护的 Node.js 库：
```javascript
const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.goto('https://example.com');
```

### Playwright
微软开源的跨浏览器自动化框架：
```javascript
const browser = await chromium.launch();
const page = await browser.newPage();
await page.goto('https://example.com');
```

## AI Agent 应用

### Agent-Browser 的 CDP 实现

| 能力 | CDP 实现 |
|------|---------|
| **网页导航** | `Page.navigate` |
| **内容提取** | `Runtime.evaluate` + DOM 查询 |
| **表单填写** | `Runtime.callFunctionOn` |
| **截图验证** | `Page.captureScreenshot` |
| **文件上传** | `Input.setInputFiles` |

## 相关实体

- [[Agent-Browser]]

## 相关概念

- [[Browser-Automation]]
- [[Web Scraping]]
