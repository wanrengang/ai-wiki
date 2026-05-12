---
title: dify
created: 2026-03-03
updated: 2026-03-03
type: entity
tags: [dify, AI-Agent, RAG, workflow, no-code]
sources: [raw/articles/2026-03-03-GitHub-AI热门项目分析.md]
---

# dify

## 项目概览

| 项目 | 内容 |
|------|------|
| **名称** | dify |
| **类型** | 可视化 AI Agent 工作流开发平台 |
| **开源协议** | Apache 2.0 |
| **技术栈** | TypeScript |
| **GitHub** | github.com/langgenius/dify |
| **Stars** | 130,964 |

## 核心定位

dify 是一个**生产级、开箱即用**的 AI 应用开发平台，核心理念是让开发者能够快速构建和部署 AI 应用，无需深入底层技术细节。

## 主要特性

### 1. 可视化工作流编排
- 拖拽式流程设计
- 支持条件分支、循环等复杂逻辑
- 内置丰富节点类型

### 2. RAG 能力
- 文档上传与切片
- 多种召回策略
- 支持混合检索

### 3. Agent 框架
- 预置多种 Agent 类型
- 支持自定义工具
- 动态指令编排

### 4. 多模型支持
- OpenAI GPT 系列
- Anthropic Claude 系列
- 开源模型（Llama、通义等）
- 一键切换

## 技术架构

```
┌─────────────────────────────────────────────────────────────┐
│                      dify Platform                           │
├─────────────────────────────────────────────────────────────┤
│  前端：React + TypeScript                                    │
│  后端：Python + FastAPI                                      │
│  数据库：PostgreSQL + Redis                                  │
│  向量库：Milvus / Qdrant / Weaviate                          │
└─────────────────────────────────────────────────────────────┘
```

## 应用场景

1. **智能客服**：基于知识库的自动问答
2. **内容生成**：营销文案、文章写作
3. **数据分析**：BI 对话式分析
4. **业务流程**：审批、文档处理自动化

## 相关概念

- [[RAG]]
- [[AI-Agent]]
- [[OpenCLAW]]
- [[langchain]]
