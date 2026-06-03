# GBrain vs Mem0 深度对比分析

> 生成时间：2026-05-25

## 基本信息

| 维度 | GBrain | Mem0 |
|------|--------|------|
| **GitHub Stars** | 18,821 | 56,658 |
| **作者** | Garry Tan（YC CEO）| 独立团队（YC 旗下）|
| **定位** | Agent 记忆操作系统 | 通用记忆层 |
| **核心创新** | 自 wiring 图谱 + 编译型情报 | 图增强记忆 + 多层次记忆 |
| **发表时间** | 2026 年 4 月 | 2025 年 4 月 |

---

## 1. 核心理念对比

### GBrain

> "Search gives you raw pages. GBrain gives you the answer."

GBrain 追求的是**直接给你答案**，而不是给你一堆页面让你自己读。

核心思想：
- **Compiled Truth + Timeline**：结论可更新，证据不消失
- **零 LLM 成本构建图谱**：typed links 是确定性规则生成
- **Synthesis Layer**：综合层不只是搜索，是读完后给你写答案

### Mem0

> "Universal memory layer for AI Agents"

Mem0 追求的是**通用的记忆基础设施**，可以嵌入任何 AI 应用。

核心思想：
- **多层次记忆**：User Memory / Session Memory / Agent Memory
- **Graph-based Memory**：用图结构捕捉实体关系
- **Single-pass ADD-only**：只追加，不更新，不删除

---

## 2. 记忆模型对比

### GBrain 的 Compiled Truth 模型

```
每个页面 = Above the fold + Below the fold

Above the fold（当前最佳结论）
  → AI 根据新证据重写
  → 用户/Agent 看到的"当前认识"

Below the fold（追加式证据链）
  → 永不编辑，只追加
  → 完整保留判断演变过程
```

**关键特性**：
- 结论会**被覆盖更新**（当新证据出现时）
- 证据链**永不消失**
- 适合需要追踪**决策演变**的场景

### Mem0 的 ADD-only 模型

```
每次对话 → LLM 提取 facts → 追加到记忆
  ↓
检索时 → 多信号融合（语义 + BM25 + 实体匹配）
  ↓
返回相关记忆片段
```

**关键特性**：
- **只追加，不更新，不删除**
- 记忆永远累积，不会覆盖
- 适合需要**完整历史**的场景

---

## 3. 知识图谱对比

### GBrain：确定性自动织网

```
写入页面 → 确定性规则提取实体 → typed links 自动生成
  works_at / invested_in / founded / advises / attended ...

成本：零 LLM 调用
结果：100% 可复现
```

**优势**：
- 构建成本极低
- 增量更新简单
- 结果完全确定

### Mem0：LLM 提取 + 图存储

```
写入 → LLM 提取实体 → 存储到图数据库（Qdrant/Chroma）
  ↓
检索时 → 向量 + 实体关系 联合查询
```

**优势**：
- 图谱更丰富（LLM 理解语义）
- 适合复杂关系推理
- 与向量检索深度融合

---

## 4. 检索/查询对比

### GBrain：综合答案 + Gap Analysis

查询返回的是**完整答案 + 引用 + 知识缺口说明**：

```
问：Alice 负责什么？
答：
"Alice 是 Acme 的工程负责人。她和 Bob 在 Q1 共同负责了
支付重构项目。目前有两个 open items：
1. 安全评审（deadline 已过）
2. 500 席报价（等待对方回复）

注意：Alice 相关的记忆从 4 月 22 日后无更新，
可能存在新的进展 brain 不知道。"
```

**核心**：不是返回相关页面，而是返回**综合后的答案** + **明确的知识缺口**。

### Mem0：多信号融合检索

```
检索信号：
1. 语义向量搜索（conceptual match）
2. BM25 关键词搜索（exact match）
3. 实体匹配（entity match）
4. 时间权重（temporal reasoning）

融合方式：RRF（Reciprocal Rank Fusion）

返回：ranked memory chunks（排序后的记忆片段）
```

**核心**：返回的是**最相关的记忆片段**，不是写好的答案。

---

## 5. 三层记忆架构对比

### GBrain

| 层次 | 存储位置 | 说明 |
|------|---------|------|
| Working | Context Window | 会话内上下文 |
| Short-term | 图谱实体页 | 跨会话短期 |
| Long-term | Postgres | 持久化存储 |

### Mem0

| 层次 | 说明 |
|------|------|
| **User Memory** | 跨用户所有会话的持久记忆 |
| **Session Memory** | 单次会话内的记忆 |
| **Agent Memory** | Agent 自身的工作状态/偏好 |

---

## 6. 技术栈对比

| 维度 | GBrain | Mem0 |
|------|--------|------|
| **存储** | PGLite（嵌入式 Postgres）| Qdrant / Chroma / PostgreSQL |
| **向量能力** | 内置 pgvector | 依赖外部向量库 |
| **图谱构建** | 零 LLM 调用（确定性规则）| LLM 提取 |
| **API 风格** | CLI + MCP | REST API + SDK |
| **部署方式** | 纯本地 | 云 / 自托管 / Library |
| **Skills 系统** | 34 个预置 Skills | 无（纯 API）|
| **Dream Cycle** | ✅ 自动优化循环 | ❌ 无 |

---

## 7. 性能对比

### Mem0 公开 Benchmark

| 测试集 | 分数 | 说明 |
|--------|------|------|
| **LoCoMo** | 91.6 | 长期对话一致性 |
| **LongMemEval** | 94.8 | 长期记忆召回 |
| **BEAM (1M)** | 64.1 | 百万 token 规模评估 |
| 相对 OpenAI 提升 | **+26%** | LLM-as-Judge 指标 |

### GBrain 公开 Benchmark

| 测试集 | 分数 |
|--------|------|
| **P@5** | 49.1% |
| **R@5** | 97.9% |
| 相对禁用图谱版本 | **+31.4 P@5** |

两者 Benchmark 不完全同质，不能直接对比分数。

---

## 8. 适用场景对比

| 场景 | 推荐 | 原因 |
|------|------|------|
| **需要直接答案** | GBrain | 综合层直接给答案 |
| **需要完整决策历史** | GBrain | Timeline 证据链 |
| **需要记忆跨用户共享** | Mem0 | User/Agent 分层更清晰 |
| **需要快速嵌入已有应用** | Mem0 | REST API + 多 SDK |
| **复杂关系推理** | 两者皆可 | 都有图谱能力 |
| **个人笔记 + AI 工作流** | GBrain | 34 Skills + Dream Cycle |
| **企业级知识库** | Mem0 | 云平台 + 自托管选项 |
| **零成本构建图谱** | GBrain | 零 LLM 调用 |

---

## 9. 核心差异总结

| 核心差异 | GBrain | Mem0 |
|---------|--------|------|
| **哲学** | 直接给答案 | 给记忆碎片，让 Agent 自己拼 |
| **图谱构建** | 零成本确定性 | LLM 语义提取 |
| **记忆更新** | 更新结论，保留证据 | 只追加不更新 |
| **交付物** | 综合答案 + Gap Analysis | 排序记忆片段 |
| **系统定位** | Agent 的"大脑" | 应用记忆层 |
| **复杂度** | 更复杂（34 Skills）| 更简单（纯 API）|

---

## 10. 选择建议

### 选 GBrain 如果：
- 你希望 AI **直接给你答案**，而不是给你一堆片段
- 你需要**追踪决策演变**（Compiled Truth 模型）
- 你需要**零成本**维护一个不断进化的知识图谱
- 你是**个人用户**，想打造个人 AI 工作流

### 选 Mem0 如果：
- 你需要把记忆能力**嵌入到产品**里（多 SDK 支持）
- 你需要**云端托管**或企业级部署选项
- 你需要**User/Session/Agent 三层分离**的清晰记忆结构
- 你倾向于**纯 API 方式**集成，不想要复杂系统

---

## 相关链接

- GBrain GitHub：https://github.com/garrytan/gbrain
- Mem0 GitHub：https://github.com/mem0ai/mem0
- Mem0 论文：arXiv:2504.19413
