# Vibe-Trading

> 港大开源的个人 AI 交易代理系统

## 概述

**Vibe-Trading** 是由 **HKUDS**（香港大学数据科学实验室）开发的开源 AI 交易代理系统，GitHub 6.6k+ Stars。

**官网**: https://github.com/HKUDS/Vibe-Trading
**Discord**: https://discord.gg/2vDYc2w5

## 规模

| 指标 | 数值 |
|------|------|
| Stars | 6.6k+ |
| Skills | 74 |
| Swarm Presets | 29 |
| Tools | 27 |
| Data Sources | 6 |

## 核心架构

将以下模块整合为统一的个人交易代理：

```
策略研究 → 回测 → 多市场数据 → 交易日志分析 → Agent Swarm → MCP工具 → Web UI
```

## 主要能力

1. **策略研究** — 量化策略开发与验证
2. **回测引擎** — 可复现的策略评估（支持 run_card 生成）
3. **多市场数据** — Tushare 等数据源集成
4. **Agent Swarm** — 多代理协作工作流
5. **MCP 协议** — Model Context Protocol 工具集成
6. **Web UI** — 交互界面

## 技术特性

- Python 3.10+
- 支持 Tushare A股基本面数据
- MCP Server 子进程测试
- CJK 字符持久化内存支持
- Trust Layer 可复现运行卡片

## 应用场景

- 量化研究
- 交易想法验证
- 历史交易复盘
- 个人量化工作流搭建

## X 平台热度

### 发布推文 (@GaryKoooo - 2026-05-10)

> 交易圈的下一个变化：每个人都能有自己的 AI Trading Desk。
> 港大 HKUDS 开源的 Vibe-Trading，把策略研究、回测、多市场数据、交易日志分析、Agent Swarm、MCP 工具和 Web UI 串成了一套个人交易代理系统。
> 6.6k 🌟 / 74 skills / 29 swarm presets / 27 tools / 6 data sources
> 适合做研究、验证想法、复盘交易、搭自己的量化工作流。

**数据**: 403 次观看、3 喜欢、3 书签

### 增长势头 (@GaryKoooo - 2026-05-11)

> 疯狂 🤯
> 引用 @0xmaz_: 1天达到80k用户，400%增长率，按此速度8天达10亿用户

### 社区反响

- 发布后1天达到 **80k 用户**
- **400%** 的增长率引发热议
- 被认为是"交易圈的下一个变化"

## GitHub 最新动态 (2026-05)

| 日期 | 更新内容 |
|------|----------|
| 05-13 | Real-price swarm grounding + worker event hygiene |
| 05-12 | Trust Layer run cards（可复现回测证据） |
| 05-11 | Memory CJK 支持 + Swarm token 精确统计 |
| 05-10 | Regression guardrails + run metadata |
| 05-09 | API path hardening + MCP 稳定性 |
| 05-08 | Tushare statement fields（财务字段筛选） |
| 05-07 | Tushare fundamentals provider |
| 05-06 | v0.1.7 发布，安全加固 |

## 社区

- Discord: https://discord.gg/2vDYc2w5
- PyPI: `pip install vibe-trading-ai`

## 来源

- GitHub: https://github.com/HKUDS/Vibe-Trading
- X @GaryKoooo (Gary Ko) — 2026-05-10
- X @0xmaz_ (Maz) — 2026-05-10

---
*最后更新: 2026-05-13*
