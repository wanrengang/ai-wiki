---
title: LLM Wiki 开源项目研究报告
date: 2026-06-04
tags:
  - AI工具
  - 知识管理
  - 开源
  - LLM
  - Karpathy
---

# LLM Wiki 开源项目研究报告

## 基本信息

| 项目 | 信息 |
|------|------|
| GitHub | [nashsu/llm_wiki](https://github.com/nashsu/llm_wiki) |
| Stars | ⭐ 10.4k |
| 定位 | "A personal knowledge base that builds itself" |
| 基于 | Karpathy LLM Wiki 方法论 |

## 核心定位

用 LLM 读取文档 → 自动构建结构化 wiki → 持续维护

**核心理念：** 传统 RAG 是"每次查询都从原始资料重新检索"，LLM Wiki 是**增量构建持久化 wiki**，知识只编译一次并持续更新，查询时读的是 wiki 而非原始资料。

## 核心技术架构（三层）

```
Raw Sources（不可变）→ Wiki（LLM生成）→ Schema（规则配置）
```

**三个核心操作：** Ingest / Query / Lint

## 核心功能详解

### 1. 两步链式思考摄取（Two-Step CoT Ingest）

| 步骤 | 内容 |
|------|------|
| Step 1 分析 | LLM 读取源文件 → 提取实体/概念/论点 → 找出与现有 wiki 的关联 → 标记矛盾 |
| Step 2 生成 | 基于分析结果生成 wiki 页面 → 含 YAML frontmatter 的源追溯 → 自动生成交叉引用 → 更新 index.md/log.md |

**增量缓存：** SHA256 哈希，文件内容不变则跳过，节省 LLM tokens

### 2. 知识图谱（4信号相关性模型）

| 信号 | 权重 | 说明 |
|------|------|------|
| 直接链接 | ×3.0 | `[[wikilinks]]` 链接 |
| 来源重叠 | ×4.0 | 共享同一原始源 |
| Adamic-Adar | ×1.5 | 共享共同邻居（按度数加权）|
| 类型亲和 | ×1.0 | 同类型页面加分（entity↔entity）|

- **Louvain 社区发现** — 自动发现知识聚类，cohesion 评分识别松散集群
- **图谱洞察** — 惊异连接（跨社区/类型）+ 知识缺口（孤立页面/稀疏社区/桥接节点）

### 3. Deep Research

- 自动搜索主题，Web 搜索（Tavily/SerpApi/SearXNG）
- 自动将研究结果入库补充 wiki
- 可一键从知识缺口触发深度研究

### 4. 多模态摄入

- PDF 内嵌图片提取
- 视觉 LLM 生成图片描述
- 图片感知搜索（带预览和来源跳转）

### 5. 查询检索流程（4阶段）

```
Phase 1: 分词搜索（支持中英文）
Phase 1.5: 向量语义搜索（可选，LanceDB，Recall 从 58.2% → 71.4%）
Phase 2: 图谱扩展（4信号相关性模型，2跳遍历+衰减）
Phase 3: 预算控制（可配置 4K~1M token 窗口）
Phase 4: 上下文组装（带编号页码，LLM 按 [1][2] 引用）
```

### 6. 本地 Agent 集成

- 内置 HTTP API：`127.0.0.1:19828`
- MCP Server 内置
- AI Agent Skill：一行命令接入 Claude Code / Codex

### 7. Obsidian 零成本迁移

整个 wiki 目录就是标准 Obsidian Vault，支持 `[[wikilink]]` 语法、YAML frontmatter、每日笔记等

## 技术栈

| 组件 | 技术 |
|------|------|
| 桌面框架 | Electron（跨平台）|
| 图谱可视化 | sigma.js + graphology + ForceAtlas2 |
| 向量数据库 | LanceDB（Rust 后端）|
| 搜索 | 支持 OpenAI 兼容 endpoint 向量搜索 |

## 与 Karpathy 原版对比

| | Karpathy 原版 | LLM Wiki 增强 |
|---|---|---|
| 形态 | CLI / 纯文档模式 | 全功能桌面应用（三栏布局）|
| 摄取 | 单步 | 两步 CoT + 增量缓存 + 队列 |
| 图谱 | 无 | 4信号相关性 + Louvain |
| 检索 | 简单读取 | 向量+图谱4阶段检索 |
| Agent | 无 | HTTP API + MCP + Agent Skill |
| 视角 | 无 | `purpose.md` 定义 wiki 目标/范围 |
| 多会话 | 无 | 完整多会话 + 对话侧边栏 |

## 适用场景

- **个人知识管理** — 让 AI 自动维护 wiki 而非每次手动检索
- **研究资料管理** — PDF/网页批量入库，自动生成知识图谱
- **知识缺口发现** — 自动识别未连接的知识区域
- **AI 工程师** — 内置 API/MCP 接入 Coding Agent
- **Obsidian 用户** — 零成本迁移，现有 Vault 直接用

## 核心创新总结

1. **从 RAG → Wiki 的范式转变**（持久化 vs 临时检索）
2. **图谱驱动的知识结构发现**（Louvain + 4信号相关性）
3. **Deep Research 闭环**（发现缺口 → 自动研究 → 补全 wiki）

## 相关截图

![[LLM_Wiki开源项目.png]]
