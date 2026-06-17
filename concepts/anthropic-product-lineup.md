---
title: Anthropic 产品矩阵全景
created: 2026-06-17
updated: 2026-06-17
type: summary
tags:
  - anthropic
  - claude
  - product
  - agent
  - summary
sources:
  - raw/articles/2026-06-17-anthropic-product-lineup.md
confidence: high
---

# Anthropic 产品矩阵全景

## 概述

Anthropic 在 2026 年的全产品矩阵调研(数据采集:2026-06-16,Anthropic 官网 Last Published)。**核心战略判断**:Anthropic 走 "AI for Problem Solvers"(官网标题)路线,与 OpenAI 押注"通用 AI 助手 + ChatGPT 神化个人用户"不同 —— 按用户场景切片,每款产品瞄准"代替人完成多步任务"而非"回答问题"。

## 4 层金字塔结构

```
                ┌─────────────────────────────────┐
                │   集成层(进入用户工作流)         │
                │   Claude for Chrome / M365 / Slack│
                └─────────────────────────────────┘
                              ▲
                ┌─────────────────────────────────┐
                │   企业垂直层                     │
                │   Claude Code Enterprise         │
                │   Claude Security                │
                └─────────────────────────────────┘
                              ▲
                ┌─────────────────────────────────┐
                │   Agent 平台层(核心战场)         │
                │   Claude Code  |  Claude Cowork  │
                │   Skills / MCP / Projects        │
                └─────────────────────────────────┘
                              ▲
                ┌─────────────────────────────────┐
                │   模型 + 接口层                   │
                │   Opus 4.8 / Sonnet 4.6 / Haiku 4.5│
                │   Claude.ai / API / SDK / Bedrock│
                └─────────────────────────────────┘
```

## 产品清单

### 模型层(2026-06 当前版本)

| 模型 | 定位 | 典型场景 |
|---|---|---|
| **Opus 4.8** | 复杂任务 + 深度研究 | 长文档综合、复杂推理、高风险决策辅助 |
| **Sonnet 4.6** | 日常主力 | 写作、快速分析、任务自动化、Agent 默认底座 |
| **Haiku 4.5** | 速度优先 | Web 搜索、日常问答、低延迟场景 |
| **Fable 5** | 未发布/内测 | 待官方公布(官网标注 "unavailable") |

### 接口层

| 产品 | 形态 | 核心价值 |
|---|---|---|
| **Claude.ai** | Web/iOS/Android | 通用聊天 + Artifacts + Projects |
| **Claude API / SDK** | API/SDK | 开发者集成,接入 Bedrock/Vertex |
| **Artifacts** | Claude.ai 内 | 聊出来的代码/图表变可分享独立页面 |
| **Projects** | Claude.ai 内 | 团队级长上下文工作区 |

### Agent 平台层(核心战场)

- **Claude Code** — CLI/IDE 扩展/VS Code 编码 Agent,全仓库自主开发
- **Claude Cowork** — 桌面应用,知识工作 Agent,操作本地文件 + 云应用
- **Skills** — 模块化能力包(Agent 时代的"插件")
- **MCP(Model Context Protocol)** — Agent ↔ 工具/数据源开放协议,Anthropic 2024 年开源

### 企业垂直层

- **Claude Code Enterprise** — 私有部署,大型代码库、合规审计
- **Claude Security** — 企业安全运营 Agent(详情待官方)

### 集成层

- **Claude for Chrome** — 浏览器内 AI 协作
- **Claude for Microsoft 365** — Office 集成
- **Claude for Slack** — 团队沟通 AI 协作

## 旗舰深度分析

### Claude Code

**定位**:Anthropic 的"agentic coding system",理解整个代码库 + 跨文件修改 + 自驱完成任务。

**官方案例数据**:

| 企业 | 部署规模 | 关键收益 |
|---|---|---|
| **Stripe** | 1370 名工程师 | 一支团队 4 天完成 10,000 行 Scala → Java 迁移(估算 10 engineer-weeks) |
| **Ramp** | 全公司 | 事故调查时间 -80%;销售/风控/财务用自然语言查数据仓库 |
| **Wiz** | 安全团队 | 50,000 行 Python → Go,约 20 小时主动开发(估算 2-3 月人工) |
| **Rakuten** | 工程师 | 新功能交付 24 工作日 → 5 天;并行多个 Claude Code session |

**核心价值**:
1. 从"AI 补全"到"AI 自主开发"
2. 降低编码门槛 — 创始人/PM/设计师/运营都能描述需求产出代码
3. 并行执行 — 多 session 同时开发,人力解放到"架构判断 + 监督"

### Claude Cowork

**定位**:知识工作 Agent,直接对标 ChatGPT / Claude.ai 聊天形态的下一代替代品。

**官方原话**:"**It is not a chat assistant**" — 明确与 Claude.ai 划清界限。

**核心价值**:
- 接手多步任务而非多步对话 — "扫描 Downloads 文件夹,分类重命名,产出报告",Cowork 直接做
- 本地文件 + 云应用操作权限 — 不是聊天框里的"知识",是真实工作流
- 触达"原本会被跳过"的繁琐工作

**典型场景**:
- 文件整理:扫描 Downloads、草稿文件夹,重命名/分类/去重
- 报告撰写:给定源文件 → 自动组装 + 综合 → 产出结构化草稿
- 数据处理:扫描销售反馈、用户调研 → 提炼模式 + 产出建议
- 研究综合:给定 topic → 跨多源文档综合

### Claude Code vs Claude Cowork 对比

| 维度 | Claude Code | Claude Cowork |
|---|---|---|
| 工作对象 | 代码仓库 | 文件系统 + 云应用 |
| 触发方式 | CLI / IDE | 桌面应用 |
| 用户群体 | 开发者 | 知识工作者 |
| 输出 | 提交代码 | 文档/报告/文件 |
| 自治度 | 高(可无人值守) | 中(需审批敏感操作) |

## 战略意图

### 三条主线

1. **企业级深耕**(Claude Code Ent / Security / M365-Slack 集成):抢 B2B 大单,绑死工作流
2. **Agent 平台化**(Claude Code / Cowork / Skills / MCP):占"Agent 操作系统"心智
3. **场景切片**(Chrome / M365 / Slack / Cowork):不押注单一入口,渗透用户所有触点

### 与 OpenAI 差异

| 维度 | Anthropic | OpenAI |
|---|---|---|
| 旗舰 C 端 | 无(Claude.ai 是订阅,不是 C 端入口) | ChatGPT(3 亿+周活) |
| 开发者 | Claude Code 旗舰 Agent | Codex 较晚入局 |
| 企业 | Claude for Work + Bedrock/Vertex | ChatGPT Enterprise + Azure OpenAI |
| Agent 协议 | **MCP(领先,生态已扩)** | 无自有标准 |
| 模型开放性 | 仅 API | API + 开源模型(gpt-oss) |
| 商业模式 | API/订阅/企业授权 | 订阅(主要)+ API |

### Anthropic 的三大赌注

> **"AI 不是更好的搜索引擎,是会干活的同事"**

1. 未来 5 年,**白领工作流会被 Agent 接管**(Cowork 是兑现物)
2. 未来 10 年,**编码会变成"提需求"而非"写代码"**(Claude Code 是兑现物)
3. Agent 时代需要**标准协议和生态**(MCP + Skills 是兑现物)

## 关键数字

- 产品总数:**12+ 款**(模型 4 + 接口 3 + Agent 5 + 企业 2 + 集成 3)
- 官方案例企业:4 家(Stripe / Ramp / Wiz / Rakuten)
- Stripe 部署规模:1370 名工程师
- 当前模型版本:Opus 4.8 / Sonnet 4.6 / Haiku 4.5(2026-06)
- 待官方公布:Fable 5 / Claude Security 详情

## 相关

- [[claude永久大脑深度研究报告]] - Claude 永久记忆方案(已有相关深度研究)
- [[claude-managed-agents]] - Managed Agents 托管平台(企业战略对照)
- [[anthropic-claude-roadmap-2026]] - Anthropic 2026 路线图
- [[anthropic-roadmap-2027]] - Anthropic 2027 ASI 路线图
- [[claude-code团队内部工作原则-2026-06-03]] - Claude Code 团队工程实践
- [[claude-金融skills-anthropic官方开源-20260510]] - Skills 实战案例
- 来源:[raw/articles/2026-06-17-anthropic-product-lineup.md](raw/articles/2026-06-17-anthropic-product-lineup.md)