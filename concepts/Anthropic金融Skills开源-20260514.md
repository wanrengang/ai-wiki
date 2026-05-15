---
title: Anthropic开源金融Skills！华尔街分析师的活装进了插件包
source: 微信公众号 - 字节笔记本
date: 2026-05-14
tags: [Anthropic, Claude, Skills, 金融, 企业Agent, Agent工作流]
---

# Anthropic开源金融Skills！华尔街分析师的活装进了插件包

**来源**: 字节笔记本  
**日期**: 2026-05-14

---

## 核心要点

Anthropic 开源了金融 Skills，可以理解为一套面向金融行业的 Claude 插件包。

### 覆盖场景

- 客户会议准备
- 行业研究
- 财报分析
- DCF/LBO/三表模型
- PE 尽调
- LP 报表审计
- 总账对账
- 月末结账
- KYC 文档审核

### 核心Agent模块

1. **Pitch Agent** - 负责可比公司、先例交易
2. **Meeting Prep Agent** - 客户会议前整理资料
3. **Market Researcher** - 围绕行业输出市场概览、竞争格局和潜在标的清单
4. **Earnings Reviewer** - 消化财报电话会和公告，更新模型并起草研报
5. **Model Builder** - 负责 DCF、LBO 和三表模型，直接在 Excel 文件里写公式

### 数据源集成

标准化财务数据、基金研究、评级数据、即时新闻、财报电话会转写、一级市场数据。

### 重要意义

这套金融 Skills 展示了**垂直行业 Agent 的组织方式**：
- 什么时候写 system prompt
- 什么时候拆 skill
- 什么时候引入 subagent
- 什么时候接 MCP 数据源
- 什么时候把风险边界写死
- 用什么方式防止模型越权

### 局限

- 数据接口是 Anthropic 写的，数据费用需自付
- 主要面向海外金融数据，不适用国内 A 股、港股场景

### 安装方式

```bash
claude plugin marketplace add anthropics/claude-for-financial-services
claude plugin install financial-analysis@claude-for-financial-services
claude plugin install pitch-agent@claude-for-financial-services
```

### 启示

这套 Skills 对于开发者来说是**企业 Agent 样板**。其他行业（医疗、政务、教育、跨境电商、企业财务、研发管理）都可以参考这个思路来重走一遍。
