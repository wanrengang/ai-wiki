---
title: mem0
created: 2026-03-03
updated: 2026-03-03
type: entity
tags: [mem0, memory, AI-Agent, long-term-memory]
sources: [raw/articles/2026-03-03-GitHub-AI热门项目分析.md]
---

# mem0

## 项目概览

| 项目 | 内容 |
|------|------|
| **名称** | mem0 |
| **类型** | AI Agent 通用记忆层 |
| **技术栈** | Python |
| **GitHub** | github.com/mem0ai/mem0 |
| **Stars** | 48,486 |

## 核心定位

mem0 专注于解决 AI Agent 的**长期记忆问题**，为 AI 系统提供跨会话、跨时间的持久化记忆能力。

## 解决的问题

| 问题 | 传统方案 | mem0 方案 |
|------|---------|----------|
| **会话丢失** | 每次会话从零开始 | 持久化记忆到下次会话 |
| **用户偏好** | 每次需要重新说明 | 自动学习用户偏好 |
| **上下文累积** | 历史记录无限膨胀 | 智能压缩和提取关键信息 |
| **多 Agent 记忆** | 各 Agent 独立 | 共享记忆层 |

## 核心特性

### 1. 多层次记忆
```
┌─────────────────────────────────────────────────────────────┐
│                    mem0 Memory Hierarchy                     │
├─────────────────────────────────────────────────────────────┤
│  用户记忆 (User Memory)                                      │
│  └─ 跨所有会话的用户偏好、习惯、关键信息                       │
│                                                              │
│  Agent 记忆 (Agent Memory)                                   │
│  └─ Agent 学到的技能、经验、决策模式                          │
│                                                              │
│  会话记忆 (Session Memory)                                    │
│  └─ 当前会话的上下文和中间状态                                │
└─────────────────────────────────────────────────────────────┘
```

### 2. 智能记忆管理
- **自动摘要**：定期压缩冗余信息
- **优先级排序**：重要记忆优先检索
- **遗忘机制**：低价值记忆自然淘汰

### 3. 跨平台支持
- Python SDK
- RESTful API
- 与 LangChain、LlamaIndex 无缝集成

## 使用示例

```python
from mem0 import Memory

# 初始化
memory = Memory()

# 存储记忆
memory.add("用户喜欢在早上处理重要任务", user_id="user123")

# 检索记忆
facts = memory.search("用户的日程习惯", user_id="user123")

# 更新记忆
memory.update("user123", facts)
```

## 相关概念

- [[AI-Agent]]
- [[RAG]]
- [[llama_index]]
