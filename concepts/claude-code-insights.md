---
title: Claude Code Insights
created: 2026-05-03
updated: 2026-05-03
type: concept
tags: [claude-code, insights, productivity, workflow, analytics]
sources: [raw/articles/Claude-Code-insights功能体验.md]
confidence: high
---

# Claude Code Insights

> Claude Code 内置的会话分析功能，生成复盘报告，发现被忽略的问题。

## 概述

`/insights` 是 Claude Code 较少被提及的功能，它分析用户的会话数据，生成纯 HTML 本地结构化报告，帮助用户复盘工作方式、发现问题和优化工作流。

## 使用方法

```
在 Claude Code 中输入 /insights
等待几分钟生成报告
```

## 个人数据示例（6天：4.28 - 5.3）

### 基础数字
- 80 个会话，810 条消息
- 每天平均 135 条消息
- 涉及 272 个文件，代码净增 21,292 行

### 工具使用排行
| 工具 | 次数 |
|------|------|
| Bash | 2,332 |
| Read | 697 |
| Edit | 451 |

**结论：** 主要用于让 Claude 跑脚本、读文件、改代码，而非聊天。

### 语言分布
- TypeScript：424 个文件
- Go：260 个
- Rust：166 个
- Python：64 个

## 发现的问题

1. **API 集成失败** — urllib 超时 → curl 沙箱限制 → http.client 阻塞，4 种方案才跑通
2. **需求描述不准确** — 双击 Esc 逻辑有歧义，折腾好几轮
3. **环境配置翻车** — Python 3.10 装的包，测试环境跑 3.14

## 改进建议

1. **重复工作流 Skill 化** — 把高频固定步骤写成 Custom Skills，如 `/publish-article`、`/github-repo`
2. **加 Hooks 前置检查** — 每次会话开始前自动跑版本检查（python3 --version、pnpm --version）
3. **API 集成预备多方案** — 提前准备好 requests、curl、MCP Server 三条路

## 核心价值

> 把平时感知不到的问题显性化，工作流 Skill 化。通过 insights 周期性复盘，不断发现和纠正可能被忽略的问题。
