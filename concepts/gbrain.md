# GBrain

> Y Combinator CEO Garry Tan 开源的 AI Agent 自织知识图谱记忆系统

## 一句话定位

**"Your AI agent is smart but forgetful. GBrain gives it a brain."**

解决 AI Agent 每次会话都是白板的问题——给它一个自织知识图谱 + 混合搜索大脑，持续学习、自动关联实体。

## 规模数据（作者自用）

- **17,888 页面**，**4,383 人**，**723 公司**
- **21 个自主 cron jobs**
- 12 天构建完成

## 性能基准

| 指标 | 分数 |
|------|------|
| **Precision@5** | **49.1%** |
| **Recall@5** | **97.9%** |
| Graph 层增益 | **+31.4 pts P@5**（对比无图谱版本）|

对比 ripgrep-BM25 + 纯向量 RAG 也有类似优势。

评测集：gbrain-evals (GitHub)

## 核心架构

```
┌──────────────────┐    ┌───────────────┐    ┌──────────────────┐
│  Brain Repo      │───>│    GBrain     │<──>│    AI Agent      │
│  (git/markdown)  │    │  (retrieval)  │    │   (read/write)   │
│  = source of     │    │  Postgres +   │    │   34 skills      │
│  truth           │    │  pgvector     │    │                  │
└──────────────────┘    └───────────────┘    └──────────────────┘
```

**核心原则：Human always wins** — 直接编辑任意 markdown 文件，`gbrain sync` 自动同步

### 存储引擎

- **PGLite**（默认）：嵌入式 Postgres，零配置，`~/.gbrain/brain.pglite`
- **Postgres**：Supabase Pro ($25/mo) 多设备同步
- 迁移命令：`gbrain migrate --to supabase|pglite`（双向）

## 知识模型

### Compiled Truth + Timeline 模式

```
---
# Above: COMPILED TRUTH（当前认知，新证据到来时重写）
# Below: TIMELINE（追加式证据链，永不编辑）
---
```

### 知识图谱 - 零 LLM 调用自动关联

写入时自动提取类型化链接（无需 LLM 调用）：

| 类型 | 触发模式 |
|------|---------|
| `attended` | 会议中提到的人 |
| `works_at` | "CEO of X" / 人物页面 |
| `invested_in` | "invested in" 模式 |
| `founded` | "founded", "co-founded" |
| `advises` | "advises", "advisor" |

**提取级联**：
1. 实体引用正则（markdown 链接 + 裸 slug）
2. 去除代码块（无假阳性）
3. 类型推断（FOUNDED → INVESTED → ADVISES → WORKS_AT）
4. 过期链接 reconciliation
5. 多类型链接约束

## 搜索管道

```
Query → Intent Classifier → Multi-Query Expansion (Haiku)
    ├── Vector Search (HNSW cosine)
    ├── Keyword Search (tsvector)
    ├── RRF Fusion: score = sum(1/(60 + rank))
    ├── Cosine Re-scoring
    ├── Compiled-Truth Boost
    ├── Backlink Boost
    └── 4-Layer Dedup
```

### Intent 路由

| 查询类型 | 处理方式 |
|---------|---------|
| Entity queries | 直接图查询 |
| Temporal queries | 时间线检索 |
| Graph traversal | 类型化关系问题 |

## 34 个 Skills

### Always-On
| Skill | 功能 |
|-------|------|
| **signal-detector** | 每条消息触发；并行 cheap model 捕获 thinking + 实体 |
| **brain-ops** | 任何外部 API 调用前先查 brain |

### 内容摄取
| Skill | 功能 |
|-------|------|
| **ingest** | 薄路由，分发到对应 skill |
| **idea-ingest** | 链接/文章/推文 → brain 页面 + 分析 |
| **media-ingest** | 视频/音频/PDF/书籍/截图 → 转录 + 实体提取 |
| **meeting-ingestion** | 会议记录 → 页面 + 与会人 + 公司时间线 |
| **voice-note-ingest** | 保留原始措辞，分发到 originals/concepts/people/companies |
| **article-enrichment** | 原始内容 → 结构化页面（含摘要、引言、关键洞察）|

### 研究与综合
| Skill | 功能 |
|-------|------|
| **book-mirror** | 旗舰功能。逐章分析，将观点映射到你的 brain |
| **strategic-reading** | 通过一个问题透镜阅读；输出：可应用的操作手册 |
| **concept-synthesis** | 去重概念存根 → 分层知识地图（T1 Canon → T4 Riff）|
| **perplexity-research** | brain 增强的网页研究；发送上下文到 Perplexity |
| **archive-crawler** | 个人文件归档（需设置 `archive-crawler.scan_paths:`）|
| **academic-verify** | 通过发表 → 方法论 → 复现追溯主张 |
| **brain-pdf** | 出版级 PDF 渲染 |

### 脑操作
| Skill | 功能 |
|-------|------|
| **enrich** | 人物/公司的分层 enrichment（T1/T2/T3）|
| **query** | 3 层搜索（综合 + 引用）|
| **maintain** | 过期页面、孤立页面、死链、引用审计 |
| **citation-fixer** | 修复缺失/格式错误的引用 |
| **repo-architecture** | 新 brain 文件的存放位置 |
| **publish** | 分享页面为密码保护的 HTML |
| **data-research** | 从邮件提取结构化数据（投资人更新、支出）|

### 运营
| Skill | 功能 |
|-------|------|
| **daily-task-manager** | P0-P3 优先级，存为 brain 页面 |

## 版本历史

| 版本 | 新特性 |
|------|--------|
| v0.25.0 | BrainBench-Real（session capture，贡献者 opt-in）|
| v0.25.1 | 研究与综合类 skills 完善 |
| v0.28.8 | LongMemEval 基准集成 |

## 安装与使用

```bash
# 安装
npm install -g gbrain

# 初始化
gbrain init

# 同步
gbrain sync

# 查询
gbrain query "your question"

# 迁移存储引擎
gbrain migrate --to supabase|pglite
```

## 与 OpenClaw/Hermes 集成

- 原生支持 OpenClaw 和 Hermes Agent
- MCP 工具暴露 30+ 接口
- Agent 每次启动从文件重建自身

## 局限与注意事项

1. **规范维护成本**：Compiled Truth 需要手动保持更新，否则与 Timeline 脱节
2. **零 LLM 调用提取**：基于正则，复杂语义可能漏提取
3. **Postgres 依赖**：生产级部署需要数据库运维
4. **学习曲线**：34 个 skills 需要时间熟悉

## 数据

- GitHub: garrytan/gbrain
- 评测: gbrain-evals
- 许可: MIT

## 来源

- GitHub: https://github.com/garrytan/gbrain
- 作者: Garry Tan（Y Combinator President/CEO）
