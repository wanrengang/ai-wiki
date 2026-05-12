---
title: RAG (Retrieval-Augmented Generation)
created: 2026-03-03
updated: 2026-03-03
type: concept
tags: [RAG, LLM, 检索增强, 知识库, AI]
sources: [raw/articles/2026-03-03-GitHub-AI热门项目分析.md]
---

# RAG (Retrieval-Augmented Generation)

## 概述

RAG（检索增强生成，Retrieval-Augmented Generation）是一种结合检索系统和生成式 AI 的混合架构，旨在解决大型语言模型的三大局限：
- **知识过时**：训练数据有截止日期
- **幻觉问题**：生成内容可能事实错误
- **私有知识缺失**：无法访问私域数据

## 核心架构

```
用户查询 → 检索系统 → 相关文档 → 提示词构建 → LLM 生成 → 回答
              ↓
         向量数据库
         (知识库索引)
```

## 技术流程

### 1. 索引阶段（Indexing）
```python
# 文档切分
documents = text_splitter.split(documents)

# 向量化
embeddings = embedding_model.encode(documents)

# 存储到向量数据库
vector_store.add_documents(documents, embeddings)
```

### 2. 检索阶段（Retrieval）
```python
# 查询向量化
query_embedding = embedding_model.encode(user_query)

# 相似度检索
results = vector_store.similarity_search(query_embedding, k=5)
```

### 3. 生成阶段（Generation）
```python
# 构建提示词
prompt = f"上下文：{results}\n\n问题：{user_query}"

# LLM 生成
response = llm.generate(prompt)
```

## 关键技术组件

### 向量数据库对比

| 数据库 | 特点 | 适用场景 |
|--------|------|---------|
| **Pinecone** | 云原生托管 | 企业级应用 |
| **Weaviate** | 混合检索 | 多模态数据 |
| **Chroma** | 轻量级 | 原型验证 |
| **Milvus** | 开源可私有 | 大规模部署 |
| **Lance** | 多模态支持 | AI Lakehouse |

### Embedding 模型

| 模型 | 维度 | 特点 |
|------|------|------|
| **text-embedding-3-large** | 3072 | OpenAI 最新 |
| **bge-large** | 1024 | 开源中文优化 |
| **m3e** | 768 | 中文场景 |

## 进阶技术

### 1. 混合检索
结合向量检索和关键词检索的优势：
- **向量检索**：语义相似度匹配
- **BM25**：关键词精确匹配
- **RRF**：倒数排名融合

### 2. 重排序（Rerank）
两阶段检索提升相关性：
1. 第一阶段：向量检索返回 Top-K
2. 第二阶段：Cross-Encoder 重排序

### 3. 自我反思（Self-RAG）
训练模型学会自己判断：
- 哪些检索结果是必要的
- 哪些生成内容需要引用
- 哪些问题应该拒绝回答

### 4. GraphRAG
基于知识图谱的 RAG：
- 提取实体和关系
- 构建知识图谱索引
- 图检索 + 向量检索

## 评测指标

| 指标 | 说明 |
|------|------|
| **Hit Rate** | 正确文档被检索到的比例 |
| **MRR** | 平均排名倒数 |
| **NDCG** | 归一化折损累积增益 |
| **RAGAS** | 专门评估 RAG 系统的指标 |

## 代表项目

| 项目 | GitHub Stars | 特点 |
|------|-------------|------|
| **ragflow** | 74,053 | 深度理解文档的 RAG 引擎 |
| **llama_index** | 47,318 | 文档 Agent 框架 |
| **anything-llm** | 55,295 | 一体化 AI 应用 |

## 相关实体

- [[ragflow]]
- [[llama_index]]
- [[dify]]

## 相关概念

- [[AI-Agent]]
- [[LLM]]
- [[Embedding]]
