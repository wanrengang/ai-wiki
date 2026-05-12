---
title: Vibe-Trading
created: 2026-05-12
updated: 2026-05-12
type: entity
tags: [person, company, open-source, agent, trading, finance]
sources: [~/mywork/code/Vibe-Trading]
---

# Vibe-Trading

## 概述

AI 驱动的多代理金融工作台，将自然语言请求转化为可执行的交易策略、研究洞见和跨全球市场的投资组合分析。

## 基本信息

- **仓库**: https://github.com/HKUDS/Vibe-Trading
- **版本**: v0.1.7
- **作者**: HKUDS (香港大学)
- **许可证**: MIT
- **Python**: 3.11+

## 核心能力

| 指标 | 数值 |
|------|------|
| Skills | 74 |
| Swarm Presets | 29 |
| Tools | 27 |
| 数据源 | 6 |
| LLM 提供商 | 13+ |

## 数据源

- Tushare (A股)
- AKShare (A股、期货)
- CCXT (加密货币)
- yfinance (港美股)
- Futu (港股、A股)
- Edgar/SEC (美股财报)

## 技术架构

- FastAPI 后端
- React 19 前端
- LangChain/LangGraph Agent 框架
- MCP (Model Context Protocol)
- Swarm 多代理编排

## 核心模块

- `agent/cli.py` - CLI 工具
- `agent/api_server.py` - API 服务
- `agent/mcp_server.py` - MCP 服务
- `agent/backtest/` - 回测引擎
- `agent/src/skills/` - 74 个技能
- `agent/src/swarm/` - 多代理编排
- `agent/src/memory/` - 持久化记忆

## 安全特性

- API 鉴权
- 路径包含校验
- Shell 工具访问控制
- Docker 非 root 运行
- 上传文件大小限制

## 安装

```bash
pip install vibe-trading-ai
vibe-trading init
vibe-trading run
```

## 相关

- [[HKUDS]] - 项目作者
- [[swarm]] - 多代理架构
- [[agent]] - AI Agent
- [[mcp]] - Model Context Protocol

---

^[[~/mywork/code/Vibe-Trading]]
