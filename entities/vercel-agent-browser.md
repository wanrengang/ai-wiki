---
title: Vercel agent-browser
created: 2026-04-08
updated: 2026-04-08
type: entity
tags: [vercel, tool, browser-automation, rust, open-source, ai-agent]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-08-Vercel-agent-browser-浏览器自动化工具.md]
---

# Vercel agent-browser

## 概述

Vercel 开源的纯 Rust 浏览器自动化 CLI 工具，专为 AI Agent 设计。Token 消耗只有 Playwright 的 6%。

## 关键数据

| 指标 | Playwright | agent-browser |
|------|-----------|---------------|
| Token 消耗（10步流程） | 114,000 | 7,000 |
| 相对比例 | 100% | **6%** |

## 核心优势

### 1. Token 消耗极低
- Playwright 解析整个 DOM 树，动辄几万 tokens
- agent-browser 只返回"可访问性树"（按钮、输入框、链接等可交互元素）

### 2. 使用极简
```bash
# Playwright（需要写代码）
await page.click('button.submit-button');
await page.fill('input[name="email"]', 'test@example.com');

# agent-browser（命令行）
agent-browser click @e2
agent-browser fill @e3 "test@example.com"
```

### 3. 无需 Node.js
纯 Rust 编译的原生 CLI，开箱即用。

## 核心功能

### 基础操作
```bash
agent-browser open example.com        # 打开网页
agent-browser snapshot -i             # 获取 AI 可读快照
agent-browser click @e2               # 点击按钮
agent-browser fill @e3 "hello"        # 填写输入框
agent-browser screenshot result.png   # 截图
agent-browser close                    # 关闭
```

### 语义定位器（AI 友好）
```bash
agent-browser find role button click --name "登录"
agent-browser find label "邮箱" fill "test@example.com"
agent-browser find text "了解更多" click
```

### 会话保持
```bash
agent-browser --session-name twitter open twitter.com
# 下次使用 session-name twitter，登录态自动恢复
```

### 安全防护
1. **域名白名单** - 只能在允许的域名操作
2. **内容边界** - 防止 AI 把网页内容当成指令
3. **操作确认** - 危险操作需人工确认

### 云端浏览器支持
```bash
-p browserless    # Browserless
-p browserbase    # Browserbase
-p kernel         # Kernel
-p browser-use    # Browser Use
-p agentcore       # AgentCore
```

## AI 工作流

1. 打开页面 + 获取快照
2. AI 从快照里找到想操作的元素（`@e1`、`@e2`、`@e3`...）
3. 执行操作
4. 重新获取快照，查看结果

循环执行，直到完成任务。

## 典型应用场景

| 场景 | 说明 |
|------|------|
| 端到端测试 | 自动化测试用户流程 |
| 自动配置 | 批量设置账号、配置选项 |
| 批量处理消息 | AI 批量读取整理消息（如 Discord 群聊） |
| 报表填写 | 自动化重复性操作 |
| 社交媒体运营 | 多平台内容发布 |

## 安装方法

```bash
# 方法一（推荐，全平台）
npm install -g agent-browser
agent-browser install  # 下载 Chrome

# 方法二（macOS）
brew install agent-browser
agent-browser install

# 方法三（Rust 用户）
cargo install agent-browser
agent-browser install
```

## 项目信息

| 项目 | 内容 |
|------|------|
| GitHub | https://github.com/vercel-labs/agent-browser |
| 作者 | Vercel Labs |
| 开源协议 | MIT |

## 与 OpenClaw 的关系

**重要发现：agent-browser 正是 OpenClaw 已集成的浏览器工具！**

- ✅ OpenClaw 的 `browser` 工具基于 agent-browser
- ✅ 支持 Snapshot 模式（AI 友好的文本结构）
- ✅ Token 消耗极低

## 相关链接

- [[openclaw-temporal-integration]] - OpenClaw 的 Temporal 集成
- [[enterprise-skill-architecture]] - 企业级 Skill 体系
- [[superpowers-claude-code]] - Claude Code 编程工作流

## 标签

#agent-browser #Vercel #浏览器自动化 #AI工具 #Rust #Playwright替代
