---
source_url: https://arxiv.org/abs/2604.04373
ingested: 2026-05-15
sha256: a1b2c3d4e5f6789012345678901234567890abcdef1234567890abcdef123456
---

# Decoction of Experience: 测试时推理经验提精术

## 核心概念

**Decoction（煎煮/提精）** = 从经验中提取本质，有组织地整理，并检索突出信息以构建有效上下文。

论文核心论点：原始经验本身是不够的——经验必须被**提精（distilled）**才能成为有用的上下文。

## 关键发现

### 1. 经验提精（Lesson Distillation）优于原始经验

从原始经验（每个问题m=4条轨迹）：
- **成功尝试**：总结关键洞察和推理模式
- **失败尝试**：反思常见错误和推理缺陷

> 通过经验提精（lesson distillation）的效果优于原始经验，在agentic任务中表现更好，并带来更好的输入可扩展性。

### 2. 记忆整合的"甜蜜点"

- 性能在**中等记忆规模时达到最优**
- 中等记忆可以**超越完整记忆**的表现
- 记忆整合（Memory Consolidation）是与经验提精并列的第二关键因素

### 3. 有效上下文需要相关性-多样性平衡

- **Relevance（相关性）**：直接适用于查询的信息
- **Diversity（多样性）**：覆盖不同问题类型和解决策略

### 4. 概念树结构

将提取的概念组织成**层级树结构**，跨概念组进行组织：
- 改进上下文构建
- 鼓励从更多样化但仍相关的概念组中检索
- 优于基线平面记忆方法

## 系统架构

### 训练阶段
```
Problems {xᵢ} → Trajectories + Rewards {oᵢ} → Raw Memory
                                    ↓
                         Memory Organization (μ)
                              ↓
                   Organized Memory M̃
```

### 测试阶段
```
New Query x → Retrieval R(x; M̃) → Context Constructor C
                              ↓
              Context c → LLM πθ → Output ŷ
```

## 实验设置

| 任务 | 训练数据 | 评估数据 | 指标 |
|------|----------|----------|------|
| 数学推理 | DAPO-Math (14,116 problems) | AMC, AIME, HMMT, BeyondAIME (260 total) | Exact-match |
| WebShop | 3,930 trajectories | 200 test episodes | Purchase quality |
| SWE-bench | 1,794 instances | SWE-bench Verified (500) | Patch passes tests |

### 关键超参数
- m = 4（每个问题的轨迹数）
- K = 8（数学推理检索数）
- α = 0.5（混合权重）
- L = 2（概念树层级）
- leaf_size = 50（数学）/ 20（WebShop）/ 10（SWE）

## 与RAG的区别

| Approach | Focus | Paper's Distinction |
|----------|-------|---------------------|
| RAG | 外部知识检索 | Agent上下文需要*策略*，不仅仅是证据 |

> Rather than increasing the output budget, one can provide the agent with carefully constructed inputs (i.e., context) to induce more efficient reasoning.
