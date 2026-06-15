---
title: AppLovin
created: 2026-06-13
updated: 2026-06-13
type: entity
tags: [company, model, inference, product]
sources: [raw/articles/2026-06-13-applovin-ai-adtech-790-percent-2024.md]
confidence: high
---

# AppLovin

## Overview

移动广告技术（Mobile AdTech）公司，创始人兼 CEO Adam Foroughi。2021 年上市，2022 年股价暴跌 92%，2024 年全年暴涨 790%（涨幅超越英伟达），2025 年市值一度突破 2500 亿美元，被纳入标普 500 指数。

**核心反差：** 没有自研大模型、没有 C 端 AI 爆款产品，却用 AI 能力驱动了超过百亿美元的效果广告预算。

## Key Facts

| Item | Detail |
|------|--------|
| CEO | Adam Foroughi |
| IPO | 2021 年 |
| 股价低点 | 2022 年，较高点跌 92% |
| 2024 涨幅 | 790%（全美股最高之一） |
| 2025 市值峰值 | ~2500 亿美元 |
| 2024 年营收 | ~45 亿美元（同比+50%） |
| 调整后 EBITDA | ~23 亿美元（利润率>50%） |
| 核心模型 | Axon（超 10 亿用户数据训练） |
| AI 团队规模 | 100+ 人（广告竞价模型研发） |

## Core AI Product: Axon

Axon 是 AppLovin 的核心 LTV（用户生命周期价值）预测神经网络，区别于传统静态标签模型的关键在于：

1. **概率分布预测**：不对用户未来付费金额做点估计，而是预测 7/14/30 天付费概率分布
2. **长周期优化**：率先实现 28 天 LTV 优化机制（已成为行业标准）
3. **被低估用户发掘**：传统模型判定为"低价值"的用户（如第 1 天只充 1 美元），Axon 通过概率分布计算出期望值 8 美元——从而以更低成本获取被竞争对手放弃的高潜用户

**飞轮效应：**
更多广告主加入 → 更多真实付费数据 → Axon 模型更好 → 广告主 ROAS 更高 → 更多广告主加入

## AI Strategy vs. Meta/Google

AppLovin 的护城河不在数据量，而在**数据之上的模型迭代速度**。Foroughi 认为：Axon 的优势是多年在真实付费数据上训练得到的神经网络飞轮，Meta 和 Google 虽有能力复制，但需要数年时间才能追上同等规模的真实付费数据训练出的效果。

## Agent Era 布局

- **开发者工具包**（2025 年初）：将 Axon 的 LTV 预测能力以 API 形式开放给第三方 App 开发者，按额外收益分成——类似 AWS 的按使用量收费模式
- **长期愿景**：把核心竞争力从"平台"转向"模型"，从"流量"转向"智能"

## Why It Matters

AppLovin 展示了一条**非模型公司用 AI 真正赚钱**的路径：当全行业追逐 LLM 概念时，AppLovin 默默把资源投入 Axon，等 LLM 泡沫消退、市场重新关注"谁在用 AI 真正变现"时，答案已经写好。

## Relationships

- [[kimi-k2-6-ai-infra]] — 同样是"AI 时代基础设施"思路，但一个做数据库基建，一个做广告智能层
- [[harness-engineering]] — AppLovin 的 Axon 本质上是一个极致优化的 AI harness：给定预算约束，最大化 LTV 产出
- [[mcp]] — AI Agent 时代广告中间商可能被绕过，AppLovin 在思考将 Axon 本身变成 Agent