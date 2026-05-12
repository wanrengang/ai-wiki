---
title: OpenClaw Web 搜索能力
created: 2026-03-08
updated: 2026-03-08
type: concept
tags: [tool, agent, search, openclaw]
sources: [raw/02AI与技术/Web搜索能力深度分析-2026-03-08.md]
confidence: high
---

# OpenClaw Web 搜索能力

## 概览

OpenClaw 2026.3.7 版本在 Web 搜索能力上有重大架构升级，核心是从 Perplexity 直连迁移到 Search API 抽象层。

## 核心变化

### Perplexity → Search API 迁移

**旧架构：** Agent → web_search tool → Perplexity API → 非结构化响应 → 需额外解析

**新架构：** Agent → web_search tool → Search API Gateway → Perplexity / Brave / Tavily → 统一结构化输出

新架构优势：
- 统一的结果格式（结构化 JSON）
- 多提供商透明切换
- 高级过滤接口一致（语言/地区/时间）
- 更好的错误处理
- 易于添加新提供商

### 结构化结果格式

```typescript
interface SearchResult {
  title: string;           // 标题
  url: string;             // 链接
  snippet: string;         // 摘要
  publishedDate?: string;  // 发布日期
  author?: string;          // 作者
  score?: number;           // 相关性评分
}

interface SearchResponse {
  results: SearchResult[];
  query: string;
  totalResults?: number;
  searchTime?: number;
}
```

## 相关实体

[[openclaw]] | [[perplexity]] | [[tavily]] | [[brave-search]]

## Tags
#tool #agent #search #openclaw
