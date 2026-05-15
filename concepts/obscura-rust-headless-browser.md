# Obscura — AI Agent 与爬虫的 Rust 轻量级无头浏览器

> 项目：https://github.com/h4ckf0r0day/obscura
> Stars：~12,297 | 语言：Rust | 最新版本：v0.1.5

## 概述

Obscura 是一个用 Rust 从零编写的无头浏览器引擎，目标明确：**替代 headless Chrome，专为 AI Agent 和爬虫场景而生**。一个月冲到 1.1 万 Stars，发展势头迅猛。

## 核心优势

| 指标 | Obscura | Headless Chrome |
|------|---------|-----------------|
| 内存占用 | **30 MB** | 200+ MB |
| 二进制体积 | **70 MB** | 300+ MB |
| 冷启动 | **瞬时** | ~2 秒 |
| 页面加载（静态） | **51 ms** | ~500 ms |
| 反检测 | **内置** | 无 |

**内存差 7 倍，页面加载快 5-6 倍。**

## 技术架构

6 个核心模块：
- `obscura-browser` — 核心浏览器引擎
- `obscura-cdp` — Chrome DevTools Protocol 实现（Puppeteer/Playwright 直连）
- `obscura-js` — V8 JavaScript 运行时嵌入
- `obscura-dom` — DOM 解析（基于 Mozilla Servo html5ever）
- `obscura-net` — 网络层（HTTP/SOCKS 代理、Cookie）
- `obscura-mcp` — Model Context Protocol 服务端（AI Agent 工具接口）

## MCP 工具集

AI Agent 可直接通过 MCP 调用浏览器操作：
`browser_navigate` / `browser_snapshot` / `browser_click` / `browser_fill` / `browser_type` / `browser_evaluate` / `browser_wait_for` / `browser_network_requests` / `browser_console_messages` / `browser_close`

## 竞品定位

- vs **Lightpanda**（Zig）：Obscura 有 MCP 原生支持，更适合 AI Agent 场景
- vs **n browser**（Playwright）：Obscura 内存少 7x，内置反检测
- vs **Headless Chrome**：资源效率碾压，但 Web API 覆盖不如完整 Chromium

## 适用场景

✅ AI Agent 浏览器操作（Claude Desktop / Cursor / Cline）  
✅ 大规模低资源爬虫（VPS 集群并发）  
✅ 隐匿爬虫（内置 stealth 模式）  
❌ 需要截图/PDF（不支持）  
⚠️ 复杂 SPA（WebGL/WebRTC 依赖）  

## 风险提示

- **版本状态**：pre-1.0，生产使用需评估
- **维护者**：单一维护者 + 商业化方向
- **反检测**：持续对抗，对 Cloudflare 高级方案可能失效

## 参考

- GitHub: https://github.com/h4ckf0r0day/obscura
- 调研日期：2026-05-15
