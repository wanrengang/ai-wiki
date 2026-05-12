---
title: Supabase
created: 2026-05-12
updated: 2026-05-12
type: entity
tags: [company, open-source, baas, postgres, ai-agent, vibe-coding]
sources: [raw/articles/2026-05-12-supabase-100b-valuation-vibe-coding-backend.md]
confidence: high
---

# Supabase

## 概述

PostgreSQL 为核心的 BaaS（Backend as a Service）平台，创立于 2020 年，以 "open-source Firebase" 和 "更简单易用的一站式 Postgres" 为价值主张。目标：让开发者 "Build in a weekend. Scale to millions"。

## 基本信息

- **估值**：100 亿美元（接近完成 GIC 领投的 5 亿美元 F 轮）
- **累计用户**：700 万+（截至 2026 年 Q1）
- **ARR**：7000 万美元+（2025 年 10 月），预计 2026 年末达数亿美元
- **投资方**：YC、Accel、Coatue、GIC
- **创始人**：Paul Copplestone（CEO）、Ant Wilson（CTO）

## 核心产品（5 层架构）

1. **Postgres 本身**：托管 Postgres + 40+ 扩展（pgvector、pg_graphql、pg_cron 等）
2. **6 个核心 BaaS 组件**：Auth / Storage / Realtime / Edge Functions / Vector / API（自动生成 REST & GraphQL）
3. **开发者 workflow**：Studio Dashboard / CLI + Branching / MCP Server / Management API
4. **企业级**：ETL / Analytics Buckets / PrivateLink / SOC 2 / HIPAA
5. **前沿 bet**：OrioleDB / Multigres / pg_duckdb / BKND Lite

## 战略定位

Supabase 同时站在两条 AI 时代大势上：
- **Postgres 作为 AI 的心智语言**：pgvector 成功，AI agent 默认推荐 Postgres
- **Coding 作为 AI 需求最大爆发点**：Opus 4.5 后 AI 从 chat 跨入 agent 时代

## 四笔收购

| 收购 | 时间 | 目的 | 技术价值 |
|------|------|------|----------|
| OrioleDB | 2024.04 | 替代存储引擎 | 解决 VACUUM/bloat，性能提升 4-5x，存储压缩 4-5x |
| Multigres | 2025.06 | 分布式分片 | 突破单机写入瓶颈，解锁 enterprise-scale |
| Hydra/pg_duckdb | 2025.12 | 列式分析引擎 | OLTP+OLAP 混合负载，分析提速数十至数百倍 |
| BKND | 2026.02 | 轻量 agent 后端 | Supabase Lite，面向 agent 的 scale-to-zero |

## Distribution Moat

- **vibe coding 平台**：Bolt、Figma、Lovable 等默认后端 + 白标服务商
- **CLI agent 推荐**：Claude Code、Codex、Cursor、Windsurf 等主动推荐（非商务合作）
- **Anthropic 官方合作**：联合推出 vibe coding platform 产品

## Enterprise 现状

- **已取得**：SOC 2 Type 2、HIPAA with BAA、ISO 27001 stage 2
- **缺口**：SCIM、Managed BYOC、PCI DSS、FedRAMP
- **Enterprise-ready 程度**：约 75%

## 竞争格局

| 竞争对手 | 类型 | Supabase 优势 |
|----------|------|---------------|
| Aurora DSQL、AlloyDB | 云厂商原生 Postgres | 开箱即用的后端能力 vs 纯数据库 |
| Neon | Serverless Postgres | 一站式 vs 需自行拼接 4 个服务 |
| Convex | 差异化 BaaS | Postgres 生态 vs Reactive 专用架构 |

## 关键风险

1. **成本结构**：Free tier 沉睡数据库侵蚀毛利，依赖 Multigres/Lite 改善
2. **Scalability**：单机 Postgres 写入 TPS 有物理上限
3. **Enterprise TAM**：目前主要用于创新探索项目，Oracle/Snowflake 对比差距 2-3 个数量级
4. **AI 发展不确定性**：若 agent 自主性提升至不依赖训练语料推荐，distribution moat 受削

## 相关概念

- [[vibe-coding]] — Supabase 的核心增长驱动之一
- [[postgres]] — 核心数据库底座
- [[baas]] — Backend as a Service
- [[orioledb]] — Supabase 收购的替代存储引擎
- [[multigres]] — Supabase 的分布式分片方案
