# GBrain：Agent 的「长期记忆操作系统」

> 来源：[公众号 盛为创新](https://mp.weixin.qq.com/s/cl4abD1DjItSvnVVBRVy2A) | 2026-05-25

## 背景

GBrain 是 Y Combinator CEO Garry Tan 开源的 Agent 记忆系统，2026 年 4 月开源，24 小时内突破 5000 Stars，目前约 18,821 Stars。

核心定位：**不是笔记工具，是编译型情报系统（compiled intelligence system）**

## 核心观点

### 1. 普通 RAG 根本不够用

| 系统 | 解决的问题 |
|------|-----------|
| **RAG** | 找得到资料 |
| **LLM Wiki** | 知识能沉淀 |
| **GBrain** | Agent 长期记忆工程化 |

RAG 的本质是"找资料"——给定 query，从大量文档里找最相关的片段。

但 Agent 真正需要的远不止"找到资料"：
- 决策延续：上次做这个决定是什么时候？依据是什么？
- 知识可信度：这条信息是最新的吗？
- 关系推理：Alice 和 Bob 是什么关系？
- 记忆积累：Agent 之前踩过什么坑？

RAG 框架里**完全不存在的**，才是 Agent 记忆的真正痛点。

### 2. 基于 Karpathy 的 LLM Wiki 理念

Karpathy 的核心观点：**Obsidian 是 IDE，LLM 是程序员，Wiki 是代码库。**

人类负责定方向、选资料、提问题，AI 负责把原始资料编译成持续维护的 Markdown Wiki。

LLM Wiki 是第一步，GBrain 在此基础上做了三次关键进化。

### 3. Compiled Truth + Timeline 架构

GBrain 把每个页面拆成两个部分：

```
Above the fold（当前最佳结论）
  → 当新证据出现时 AI 自动重写

Below the fold（追加式证据链）
  → 永不编辑，只追加
```

**解决了 Agent 最大的痛点：不是找不到资料，而是不知道该相信什么。**

项目方案迭代、决策变更时，传统 Wiki 把新旧信息混为一谈；GBrain 让「结论可更新、证据不消失」，变成可追溯的决策系统。

### 4. 三层记忆结构

| 层次 | 特点 | GBrain 对应 |
|------|------|-----------|
| **Working** | 当前上下文，窗口内 | 会话 Context |
| **Short-term** | 跨会话，短期 | 图谱中的实体页 |
| **Long-term** | 持久化，长期 | Postgres 存储 |

**核心洞察**：长期记忆的核心不是「记得多」，而是「记得准、记得干净」。

### 5. 自动织网（零 LLM 成本）

GBrain 实现 The brain wires itself：每次写入自动抽取实体，创建 typed links（类型化关系）：`works_at`、`invested_in`、`founded`、`advises`……

把原本需要人工整理的关系，变成系统原生能力。

### 6. 自 wiring 知识图谱

- **零 LLM 调用**构建图谱，成本极低
- **完全可复现**：同样输入永远同样输出
- **支持增量更新**：只重新分析变化的文件

性能数据：
- P@5：**49.1%**
- R@5：**97.9%**
- 相比禁用图谱版本，P@5 高出 **+31.4 分**

## 技术架构

### 存储层

- **PGLite**：嵌入式 Postgres，无需独立服务器
- 安装 30 分钟，数据库 2 秒就绪
- 完全本地化，数据在自己硬件上

### 34 个 Skills

| 类别 | Skills |
|------|--------|
| **常驻** | signal-detector、brain-ops |
| **内容摄取** | ingest、idea-ingest、media-ingest、meeting-ingestion、voice-note-ingest |
| **研究与综合** | book-mirror、strategic-reading、concept-synthesis、perplexity-research |

### Dream Cycle

每天夜里自动：
- 修复引用错误
- 合并记忆碎片
- 丰富实体信息

用户醒来时，brain 比睡前更聪明。

## 与 LLM Wiki 的关系

| 对比 | LLM Wiki | GBrain |
|------|----------|--------|
| **形态** | Markdown + Git | Postgres + PGLite |
| **规模** | 有限 | 可扩展到 10 万+ 页 |
| **图谱** | 手动 wikilinks | 自动 wiring |
| **增量更新** | 困难 | 原生支持 |

## 核心价值

> **Search gives you raw pages. GBrain gives you the answer.**

- 搜索给你一堆页面，GBrain 直接给你带引用的答案
- 明确告诉你"这 brain 不知道什么"（Gap Analysis）
- 支持关系推理，向量搜索永远回答不了的问题

## 相关链接

- GitHub：https://github.com/garrytan/gbrain
- 官网：https://gbrain.homes/
- Stars：18,821（截至 2026-05-25）
