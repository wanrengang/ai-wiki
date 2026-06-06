---
title: gbrain — AI Agent 知识脑
created: 2026-05-03
updated: 2026-05-03
type: entity
tags: [AI, knowledge-graph, agent, openclaw, hermes-agent, gbrain, rag]
sources: [https://github.com/garrytan/gbrain]
confidence: medium
---

# gbrain

## 一句话定位

> **"Your AI agent is smart but forgetful. GBrain gives it a brain."**
> — Garry Tan（Y Combinator 总裁兼CEO）

Garry Tan 的个人 AI Agent 生产脑，驱动着他的 OpenClaw 和 Hermes 部署：**17,888 页面，4,383 人，723 公司，21 个 cron 作业自律运行**，12天构建完成。Agent 在你睡觉时摄取会议、邮件、推文、语音通话和原创想法，丰富每个遇到的人和公司，自动修复引用，夜间整合记忆。醒来时大脑比睡前更聪明。

## 核心架构

### 自织知识图谱（Self-Wiring Knowledge Graph）
- 每次写页面时，提取实体引用并创建**类型化链接**：`attended`, `works_at`, `invested_in`, `founded`, `advises`
- **零 LLM 调用**完成图谱编织
- 混合搜索：向量 + BM25 + 图谱backlink增强排序

### 基准测试（BrainBench）
在 240 页 Opus 生成的丰富文本语料上：
- **P@5: 49.1%**，**R@5: 97.9%**
- 超越自身禁用图谱版本 **+31.4 P@5**
- 超越 ripgrep-BM25 + 纯向量 RAG 幅度类似
- 图谱层 + v0.12 提取质量共同承担差距

### 数据规模
- **17,888** 页面
- **4,383** 人（person pages）
- **723** 公司（company pages）
- **21** cron 自主作业
- 构建时间：**12天**

## 34 个 Skills

### Always-on（常驻）
| Skill | 功能 |
|-------|------|
| signal-detector | 每条消息触发，并行廉价模型捕获原始思维和实体提及 |
| brain-ops | 任何外部API调用前先做知识脑查询，读-丰富-写循环 |

### 内容摄取
| Skill | 功能 |
|-------|------|
| ingest | 薄路由，检测输入类型并委托 |
| idea-ingest | 链接/文章/推文变为脑页面，含分析、作者人物页面、交叉链接 |
| media-ingest | 视频/音频/PDF/书籍/截图/GitHub仓库，自动转录+实体提取 |
| meeting-ingestion | 会议转录变脑页面，每个参与者被丰富，每公司生成时间线条目 |
| voice-note-ingest | 保留原始措辞，从不转述 |
| article-enrichment | 原始文章变结构化页面，含执行摘要、原话引用、关键洞察 |

### 研究与综合（v0.25.1）
| Skill | 功能 |
|-------|------|
| book-mirror | flagship。给Agent一本书，得到个性化双栏分析 |
| strategic-reading | 通过单一问题透镜阅读书籍/文章/案例研究 |
| concept-synthesis | 将数千概念存根去重为分层知识图（T1 Canon → T4 Riff）|
| perplexity-research | 知识脑增强的网页研究，避免已知信息，专注新内容 |
| archive-crawler | 通用档案管理员（Dropbox/Backblaze/Gmail/硬盘）|
| academic-verify | 追溯研究声明：出版物→方法论→原始数据→独立复现 |

### 脑操作
| Skill | 功能 |
|-------|------|
| enrich | 分层丰富（T1/T2/T3），创建更新人物/公司页面 |
| query | 3层搜索+综合+引用，不知道时说"脑里没信息"而非幻觉 |
| maintain | 定期健康检查：陈旧页面、孤儿、断链、引用审计 |
| citation-fixer | 扫描并修复缺失/格式错误的引用 |
| repo-architecture | 新脑文件去向决策协议 |

### 操作
| Skill | 功能 |
|-------|------|
| daily-task-manager | 任务生命周期（P0-P3），存为可搜索脑页面 |
| daily-task-prep | 晨间准备：日历前瞻+每人脑背景+开放线程+任务审查 |
| cron-scheduler | 交错调度+静默小时+幂等性 |
| reports | 时间戳报告+关键词路由 |
| minion-orchestrator | 后台工作编排，shell作业+LLM子Agent，父子DAG |

## 与 OpenClaw / Hermes 的关系

### OpenClaw（原生支持）
gbrain README 明确推荐 **OpenClaw** 作为部署平台：
> **[OpenClaw](https://openclaw.ai)** ... Deploy [AlphaClaw on Render](https://render.com/deploy?repo=https://github.com/chrysb/alphaclaw)（一键，8GB+ RAM）

安装命令：
```bash
Retrieve and follow the instructions at:
https://raw.githubusercontent.com/garrytan/gbrain/master/INSTALL_FOR_AGENTS.md
```
Agent 自动克隆、安装、配置、加载34个skills。

### Hermes（通过 bridge 连接）
有第三方桥接工具：**[hermes-gbrain-bridge](https://github.com/howardpen9/hermes-gbrain-bridge)**（28 ⭐）

功能：将 Hermes / OpenClaw agent 记忆（JSONL sessions, MEMORY.md）转换为 markdown 供 gbrain 摄取。

支持格式：
| 工具 | 路径 | 格式 |
|------|------|------|
| Hermes | `~/.hermes/sessions/*.jsonl` + `memories/*.md` | JSONL + `§`-分隔markdown |
| Claude Code | `~/.claude/projects/<path-hash>/*.jsonl` | 事件流JSONL |
| Codex CLI | `~/.codex/sessions/**/*.jsonl` | JSONL + session_meta |
| OpenClaw archive | `~/.openclaw.pre-migration/workspace/**/*.md` | Markdown |

架构：
```
~/.hermes/sessions/*.jsonl  ──┐
~/.hermes/memories/*.md     ──┤
~/.claude/projects/**/*.jsonl─┼→ hermes-gbrain-bridge → /tmp/gbrain-staging/*.md → gbrain import
~/.codex/sessions/**/*.jsonl─┤
~/.openclaw.pre-migration/** ──┘
```

### DATACLAW-AGENT（OpenClaw + gbrain + 数据科学）
> A personal data scientist that runs on mac mini built ontop of openclaw; Dataclaw = OpenClaw + Gbrain + Custom data-science Harness

## 与 Karpathy LLM Wiki 的区别

| 维度 | Karpathy LLM Wiki | gbrain |
|------|-------------------|--------|
| 作者 | Andrej Karpathy | Garry Tan (YC CEO) |
| 架构 | 纯 Markdown + 手动双链 | PGLite/Postgres + pgvector + 类型化知识图谱 |
| 搜索 | 关键词 + 向量 | 混合搜索 + 图谱walk |
| 自动化 | 手动 ingest | 34 skills + 21 cron自主运行 |
| 链接类型 | 无类型双链 | 类型化关系（attended, works_at...）|
| 规模 | ~57页面 | 17,888页面 |
| 基准测试 | 无 | P@5 49.1%, R@5 97.9% |

## 安装路径

### 方式1：让 Agent 自动安装（推荐）
让 OpenClaw 或 Hermes Agent 执行：
```
Retrieve and follow the instructions at:
https://raw.githubusercontent.com/garrytan/gbrain/master/INSTALL_FOR_AGENTS.md
```
~30分钟，Agent自动完成。

### 方式2：独立CLI
```bash
git clone https://github.com/garrytan/gbrain.git && cd gbrain
bun install && bun link
gbrain init   # 本地脑，2秒就绪
gbrain import ~/notes/
gbrain query "what themes show up across my notes?"
```

### 方式3：通过 hermes-gbrain-bridge 连接已有 Hermes
```bash
git clone https://github.com/howardpen9/hermes-gbrain-bridge
cd hermes-gbrain-bridge && bun install
bun run src/cli.ts discover --days 30      # 先看看有什么
bun run src/cli.ts export --source=all --out=/tmp/gbrain-staging --days 30
gbrain import /tmp/gbrain-staging --no-embed
gbrain embed --stale
```

## 技术栈
- **语言**：TypeScript
- **数据库**：PGLite（本地，默认）/ Postgres + pgvector（云端）
- **CLI**：Bun
- **认证**：OAuth 2.1（MCP HTTP模式）
- **Stars**：12,863 ⭐

## log
- 2026-05-03：从 GitHub 研究并存入 Wiki | stars: 12,863 | Y Combinator CEO Garry Tan 个人生产脑
