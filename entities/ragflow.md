---
title: ragflow
created: 2026-03-03
updated: 2026-03-03
type: entity
tags: [ragflow, RAG, deepdoc, document-understanding]
sources: [raw/articles/2026-03-03-GitHub-AI热门项目分析.md]
---

# ragflow

## 项目概览

| 项目 | 内容 |
|------|------|
| **名称** | ragflow |
| **类型** | 深度文档理解 RAG 引擎 |
| **技术栈** | Python |
| **GitHub** | github.com/infiniflow/ragflow |
| **Stars** | 74,053 |

## 核心定位

ragflow 是一个专注于**深度文档理解**的 RAG（检索增强生成）引擎，旨在解决传统 RAG 在复杂文档（如表格、图表、多页文档）处理上的局限性。

## 核心技术：DeepDoc

### 文档解析能力

| 能力 | 说明 |
|------|------|
| **布局分析** | 识别文档结构（标题、段落、表格、图表） |
| **表格识别** | 精准提取表格结构化数据 |
| **图表理解** | 解析图表中的数据和关系 |
| **多语言支持** | 中英文混合文档处理 |

### 切片策略

| 策略 | 适用场景 |
|------|---------|
| **标题分段** | 结构清晰的文档 |
| **段落切片** | 长文本内容 |
| **表格切片** | 包含大量表格的文档 |
| **滑动窗口** | 需要上下文重叠的场景 |

## 与 dify 的关系

- **dify**：偏向工作流编排和 Agent 开发
- **ragflow**：专注于文档理解和检索增强
- 两者可互补使用：ragflow 提供 RAG 能力，dify 负责应用编排

## 相关概念

- [[RAG]]
- [[dify]]
- [[DeepDoc]]
