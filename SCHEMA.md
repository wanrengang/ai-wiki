# Wiki Schema

## Domain
AI/ML 研究、前沿模型、AI Agent 技术、工程落地实践

## Conventions
- 文件名：小写、中划线、无空格（`transformer-architecture.md`）
- 每个 wiki 页面以 YAML frontmatter 开头
- 使用 `[[wikilinks]]` 建立页面间链接，每个页面至少 2 个外部链接
- 更新页面时必须更新 `updated` 日期
- 新页面必须添加到 `index.md` 对应章节
- 所有操作必须追加到 `log.md`
- 综合 3+ 来源的页面，在相关段落末尾加 `^[raw/articles/source-file.md]` 溯源标记
- **溯源标记：** 单来源页面用 `sources:` frontmatter 即可，综合多来源页面用 `^[...]` 标记每段

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
# Optional quality signals:
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

## Tag Taxonomy
- 模型：model, architecture, benchmark, training, fine-tuning
- 人物/组织：person, company, lab, open-source
- 技术：optimization, inference, alignment, data, training
- 应用：agent, coding, reasoning, multimodal, rag
- 领域：research, product, engineering, news
- Meta：comparison, timeline, controversy, prediction, tutorial

## Page Thresholds
- **创建页面：** 实体/概念在 2+ 来源中出现，或在一个来源中处于核心地位
- **追加到现有页面：** 来源提及已覆盖的内容
- **不创建页面：** 仅一次提及、细枝末节、或超出领域范围的内容
- **拆分页面：** 超过 ~200 行时拆分为子主题
- **归档页面：** 内容完全被取代时移至 `_archive/`，从 index 移除

## Entity Pages
每个 notable 实体一个页面，包含：
- 概述/是什么
- 关键事实和日期
- 与其他实体的关系（[[wikilinks]]）
- 来源引用

## Concept Pages
每个概念/主题一个页面，包含：
- 定义/解释
- 当前认知状态
- 开放问题或争议
- 相关概念（[[wikilinks]]）

## Comparison Pages
对比分析，包含：
- 比较对象和原因
- 对比维度（表格格式优先）
- 结论或综合判断
- 来源

## Update Policy
新信息与现有内容冲突时：
1. 检查日期 — 新来源通常取代旧来源
2. 确实矛盾时，同时记录双方立场和日期来源
3. 在 frontmatter 标记：`contradictions: [page-name]`
4. 在 lint 报告中标记待用户审核

## Raw Frontmatter
```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest>
---
```
