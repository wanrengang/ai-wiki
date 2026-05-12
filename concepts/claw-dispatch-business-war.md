---
title: Claude Dispatch 与 OpenClaw 商战分析
created: 2026-03-24
updated: 2026-03-24
type: concept
tags: [anthropic, claude, openclaw, dispatch, business-war, security]
sources: [raw/articles/Claude-Dispatch与OpenClaw商战分析.md]
confidence: high
---

# Claude Dispatch 与 OpenClaw 商战分析

> Anthropic 30天四连击，Claude 亲手下场"杀死"OpenClaw 的无摩擦幻想。

## 核心事件

**Anthropic 发布 Dispatch**：手机控制电脑的 Agent 工具
- 定位：手机到桌面的持久线程
- 价格：Max 订阅用户（月费 $100 起）
- 目前仅 macOS，Windows/Linux 筹备中
- 架构：建立在 Claude Code CLI 工具之上的远程控制层

## Anthropic 的 30 天四连击

| OpenClaw 能力 | Claude 回应 |
|---------------|-------------|
| WhatsApp 消息代理 | **Dispatch**：手机到桌面持久线程 |
| Discord/Telegram 控制 | **Claude Code Channels**：MCP 协议桥接 |
| 操作系统/浏览器控制 | **Computer Use + Claude Code**：共享计算机使权 |
| 24小时守护进程 | **无**（要求用户在场） |

## 核心差异

| 维度 | OpenClaw | Claude |
|------|----------|--------|
| 设计理念 | 无摩擦、24/7 无人值守 | 有护栏、用户在场 |
| 安全 | CVE-2026-25253（8.8分漏洞） | 企业级安全合规 |
| 后台运行 | ✅ 支持 | ❌ 不支持 |
| 暴露公网 | 是 | 否 |

## OpenClaw 安全危机

### CVE-2026-25253 漏洞

- 发现者：Oasis Research
- 严重性：8.8/10
- 影响：任何网站可静默接管用户的 OpenClaw Agent
- 特点：无需插件、无需扩展、无需用户交互

### 技能市场恶意软件

- 12% 的技能被确认为恶意软件
- 伪装成交易机器人、财务助手
- 实际窃取密码、加密钱包、云服务凭证

## "App 会消失"叙事的反思

### 原有叙事（Karpathy、OpenClaw 创始人）
- AI Agent 将替代 80% 的 App
- GUI 已死，CLI 永生

### Anthropic 的回应：App 不会消失

**证据：**
1. Dispatch 产品架构中 App 在最顶层（控制台）
2. 用户行为数据：老用户（750+会话）40% 自动模式，但中断率同步上升

**结论：** 信任不是"放手"，而是"知道何时介入"

## App 的三次生命

| 次数 | 代表 | 用法 | 本质 |
|------|------|------|------|
| 第一次 | 计算器、天气 | 打开→使用→关闭 | 工具 |
| 第二次 | 社交媒体、短视频 | 上瘾、刷不停 | 成瘾 |
| **第三次** | **Claude Dispatch** | 打开→告诉AI做什么→检查结果 | **控制面板** |

## GUI 不会消失的三个原因

1. **自动驾驶类比**：机器决策时，人需要可感受、可介入、可推翻的控制点
2. **自然语言带宽限制**：高密度信息输出需要视觉界面，而非语音朗读
3. **计算史分层规律**：新技术不杀死旧事物，而是让其迭代

## 对 Agent 开发的启示

- **安全优先于便利**：无摩擦 → 无安全
- **GUI 是控制面板，不是执行层**
- **CLI-Anything 验证了执行层价值**

## 结论

1. Dispatch 不是杀死 OpenClaw，而是杀死"无摩擦"的幻想
2. App 不会消失，而是升级为 AI 指挥台
3. CLI-Anything 的价值被验证：执行层的 CLI 生成器
