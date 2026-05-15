---
title: Claude 金融 Skills（Anthropic 官方开源）
description: Anthropic 官方开源的金融领域 Agent Skills 仓库，覆盖投行、股票研究、私募股权、财富管理
type: concept
tags:
  - Claude
  - Skills
  - Anthropic
  - 金融AI
  - Agent
  - MCP
created: 2026-05-14
source: https://mp.weixin.qq.com/s/8_S8ynPyy7SHy_OW0lbYuA
author: 老章（Ai学习的老章）
---

# Claude 金融 Skills（Anthropic 官方开源）

## 概述

Anthropic 官方开源了 `claude-for-financial-services` 仓库（Apache 2.0），把华尔街分析师的整套工作流做成了可装的插件包。

> 官方定位：替分析师起草工作底稿（模型、备忘录、研报、对账单），**不做投资决策、不执行交易、不绑定风险、不批准开户**，每一份产出都摆在那儿等人类签字。

## 仓库结构

- **Agents（11个）**：端到端工作流智能体，独立打包
- **Vertical Plugins（7+2个）**：底层 Skill / 斜杠命令 / 数据连接器

## 11个 Agent

| Agent | 业务方向 | 功能 |
|---|---|---|
| Pitch Agent | 客户与咨询 | 可比公司 + 先例交易 + LBO → 带品牌 pitch deck |
| Meeting Prep Agent | 客户与咨询 | 客户会议前自动出 briefing pack |
| Market Researcher | 研究与建模 | 行业概览 + 竞争格局 + peer comps + 标的清单 |
| Earnings Reviewer | 研究与建模 | 财报电话会 → 更新模型 → 起草研报 |
| Model Builder | 研究与建模 | **DCF、LBO、三表模型直接在 Excel 里跑** |
| Valuation Reviewer | 基金运营 | GP 报送包 → 跑估值模板 → LP 报告 |
| GL Reconciler | 基金运营 | 找总账 break、追根溯源、走签字流程 |
| Month-End Closer | 基金运营 | 月末结账：计提、滚存、差异说明 |
| Statement Auditor | 基金运营 | LP 报表分发前审计 |
| KYC Screener | 运营与开户 | 解析开户文档 + 跑规则引擎 + 标记缺口 |

## 核心 Skill 一览

### financial-analysis（核心建模）
`/comps`（可比公司分析）、`/dcf`（DCF 估值 + WACC + 敏感性分析）、`/lbo`（杠杆收购模型）、`/3-statement-model`（三表模型填充）、`/debug-model`（Excel 审计——公式追溯、硬编码检测）

### investment-banking（投行）
`/cim`、`/teaser`、`/buyer-list`、`/merger-model`、`/process-letter`、`/deal-tracker`

### equity-research（卖方研究）
`/earnings`、`/earnings-preview`、`/initiate`、`/morning-note`、`/sector`、`/thesis`、`/catalysts`、`/screen`

### private-equity（私募股权）
`/source`、`/dd-checklist`、`/unit-economics`、`/ic-memo`、`/value-creation`、`/ai-readiness`

### wealth-management（财富管理）
`/client-review`、`/financial-plan`、`/rebalance`、`/tlh`

## 11个 MCP 数据连接器

Daloopa、Morningstar、S&P Global + Capital IQ、FactSet、Moody's、MT Newswires、Aiera、LSEG、PitchBook、Chronograph、Egnyte

> ⚠️ 大部分需要机构席位订阅

## 安装

```bash
claude plugin marketplace add anthropics/claude-for-financial-services
claude plugin install financial-analysis@claude-for-financial-services
```

## 意义

1. **不是产品，是参考实现** — 按公司方式调整后才能用
2. **Anthropic 在立行业标准** — 医疗、法律、咨询、政务会有类似仓库
3. **是最好的 Skill 写作教科书**
