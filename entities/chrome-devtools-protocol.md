---
title: Chrome DevTools Protocol
created: 2026-02-06
updated: 2026-02-06
type: entity
tags: [browser, automation, CDP, MCP, AI-tools]
sources: [raw/articles/2026-02-06-Chrome DevTools Protocol技术调研报告.md]
---

# Chrome DevTools Protocol

## 概述

Chrome DevTools Protocol (CDP) 是 Chrome 浏览器调试工具的核心通信协议，基于 JSON 格式，通过 WebSocket 实现客户端与浏览器内核之间的双向实时交互。AI 时代正在成为连接 AI 智能与浏览器能力的桥梁。

## 核心能力

CDP 提供了约 300+ 个命令，按功能划分为多个域（Domains）：

| 域 | 功能 |
|----|------|
| **Page** | 页面导航、截图、PDF生成、脚本执行 |
| **Network** | 网络请求拦截、监控、模拟、修改 |
| **DOM** | DOM树查询、修改、事件监听 |
| **Runtime** | JavaScript执行、对象检查 |
| **Console** | 控制台日志读取、消息监听 |
| **Performance** | 性能追踪、指标收集 |
| **Emulation** | 设备模拟、网络条件模拟、地理位置模拟 |
| **Security** | 证书信息、安全状态 |
| **Storage** | Cookie、LocalStorage、SessionStorage管理 |

## 工作原理

```
┌─────────────┐         WebSocket/HTTP         ┌──────────────┐
│   客户端     │ ←─────────────────────────→  │ Chrome浏览器   │
│  (CDP客户端) │    命令/事件的双向通信         │  (CDP服务端)   │
└─────────────┘                              └──────────────┘
```

## 生态系统

基于 CDP 构建的知名开源项目：

- **Puppeteer**：Google 官方维护的 Chrome 自动化工具
- **Playwright**：微软推出的跨浏览器自动化测试框架
- **Selenium 4**：集成 CDP 支持的传统测试工具
- **chrome-devtools-mcp**：Google 官方的 MCP 服务器

## AI 时代的意义

### 范式转变

**传统模式**：预定义规则，盲盒式操作
**AI 增强模式**：理解上下文并决策，赋予 AI "运行时感知能力"

### 核心突破

Chrome DevTools MCP 让 AI 助手能够：
- 直接观察代码执行后的浏览器状态
- 捕获性能追踪数据
- 读取控制台错误日志
- 监控网络请求和响应
- 执行 JavaScript 并检查结果

## MCP 集成

Chrome DevTools MCP Server 是 Google 官方提供的 MCP 服务器，让 AI 助手能够直接连接到 Chrome 浏览器的调试能力。

主要工具：
- `chrome-devtools_navigate_page` - 页面导航
- `chrome-devtools_click` - 点击元素
- `chrome-devtools_fill` - 填写表单
- `chrome-devtools_take_snapshot` - 获取页面快照
- `chrome-devtools_take_screenshot` - 截图
- `chrome-devtools_evaluate_script` - 执行 JavaScript
- `chrome-devtools_performance_start_trace` - 性能追踪

## 相关概念

- [[Agent-Browser]]
- [[MCP]]
- [[Browser-Automation]]
