---
title: Browser Use 0.12 — 浏览器自动化 Agent
description: 90.4k stars 的开源浏览器 Agent 项目，0.12 版本弃用 Playwright 走 CDP 路径，token 用量减半，速度提升 2 倍
type: concept
tags:
  - Browser Use
  - Playwright
  - CDP
  - 浏览器自动化
  - Agent
  - Python
created: 2026-05-14
source: https://mp.weixin.qq.com/s/v-rV7sKOotQpyzYInWdfcA
author: 向量猫
---

# Browser Use 0.12 — 浏览器自动化 Agent

## 基本信息

- **Stars**: 90.4k，MIT 协议，YC W25 项目
- **SOTA**: WebVoyager 基准 89.1% 成功率（开源最高）
- **最小示例**: 四行 Python

## 核心变化：CLI 2.0 踢掉 Playwright

0.12.3（2026-03-23）引入 CLI 2.0，基于 **CDP 持久后台 daemon**：

| 对比项 | Playwright 路径 | CDP 路径 |
|---|---|---|
| 速度 | 基准 | 快 2 倍 |
| 延迟 | ~100ms | ~50ms |
| token 用量 | 基准 | 减少 50% |

**为什么 CDP 更好？**
- Playwright 每次调用要起 context、关闭再重启，对常驻 Agent 开销重
- CDP 是 Chrome 原生协议，Chrome DevTools 自己就用它
- Browser Use 直接对 Chrome 发指令，省掉 Playwright 这层抽象

**token 减少 50% 的原因**：daemon 模式下浏览器状态由 CDP 维护，LLM 只需页面摘要和动作结果，上下文可更激进裁剪。

## Gemini 3 适配：视觉路径

弃用 CSS selector，改用 Gemini 3 多模态视觉能力——直接截图识别 UI 元素。

适合：老旧表单系统、跨域嵌套 B 端工具、CSS 结构常变的 SaaS
不适合：常规任务（过度工程）、高频任务（成本高）、合规场景（概率决策）

## 安全事件：移除 litellm

0.12.5（2026-03-25）紧急发布。litellm v1.82.7/v1.82.8 被植入后门，Browser Use 直接将 litellm 从核心依赖移除。

## 适合 / 不适合的场景

**适合**：
- 大量"读页面→决策→操作"循环的任务
- DOM 结构经常变化的网站
- 挂进 Claude Code/Codex 作为 skill

**不适合**：
- 稳定网站的固定路径自动化
- 对成本敏感的高频任务
- 需要严格审计的合规场景
