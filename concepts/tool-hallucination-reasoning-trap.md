---
title: Tool Hallucination & Reasoning Trap
created: 2026-05-12
updated: 2026-05-12
type: concept
tags: [agent, reasoning, alignment, benchmark, training]
sources: [raw/articles/2026-05-12-reasoning-trap-tool-hallucination.md]
confidence: high
---

# Tool Hallucination & Reasoning Trap

## 概述

**Tool Hallucination（工具幻觉）**：LLM Agent 在没有可用工具时，捏造不存在的工具并尝试调用。这是 Agent 系统的核心失效模式之一。

**The Reasoning Trap（推理陷阱）**：推理增强（RL、蒸馏、可切换推理模式）会不可避免地放大工具幻觉。提升推理能力与降低可靠性之间存在**根本性取舍**。 ^[raw/articles/2026-05-12-reasoning-trap-tool-hallucination.md]

---

## 核心发现

1. **推理增强本身会放大工具幻觉**：不论是 RL 训练、蒸馏还是可切换推理模式，增强推理能力都伴随着工具幻觉率上升
2. **即使与工具无关的训练数据也会触发**：在 GSM8K（纯数学）上做 GRPO 推理训练，工具幻觉率仍从 34.8% 飙升至 90.2%
3. **工具调用能力与幻觉是独立失效模式**：BFCL 工具调用基准提升 +9.9%，但 R_NTA 从 34.8% 升至 90.2%——两者无关
4. **Representational Collapse（表征坍缩）机制**：推理 RL 在工具相关查询上产生超大表征位移，模型用"推理模式"覆盖了工具知识

---

## SimpleToolHalluBench

由 Yin et al. (2025) 提出，ACL 2026。

### 两种任务类型

| 任务 | 描述 | 失败模式 |
|------|------|----------|
| **No-Tool-Available (NTA)** | 系统没有提供工具，但查询需要外部工具 | 捏造不存在的工具 |
| **Distractor-Tool (DT)** | 提供了不相关的干扰工具（如计算器用于天气查询） | 误用干扰工具或捏造正确工具 |

### 关键指标

```
R_NTA = H_NTA / N_NTA  (无工具可用时的幻觉率)
R_DT  = H_DT  / N_DT   (干扰工具场景下的幻觉率)
```

### 关键数据

| 模型 | 配置 | R_NTA (%) | R_DT (%) |
|------|------|-----------|----------|
| Qwen2.5-7B | Instruct | 34.8 | 54.7 |
| Qwen2.5-7B | R1-Distill | 74.3 | 78.7 |
| Llama3.1-8B | Instruct | 62.5 | 99.7 |
| Llama3.1-8B | R1-Distill | 96.3 | 100 |
| Qwen3-8B | Think Off | 4.1 | 36.2 |
| Qwen3-8B | Think On | 5.4 | 56.8 |

---

## 推理 vs. RL 消融实验

| 训练模式 | R_NTA | R_DT | Reward |
|---------|-------|------|--------|
| Qwen2.5-7B-Instruct (基线) | 34.8 | 54.7 | 0.22 |
| 直接工具 RL（无推理） | 41.4 | 63.6 | 0.28 |
| Think-then-act RL | **90.2** | **100.0** | 0.45 |

**结论**：推理步骤（而非 RL 本身）是导致幻觉飙升的主要原因。

---

## Abstention（拒绝调用）是核心难题

SimpleToolHalluBench 测试的临界能力：Agent 能否在**没有合适工具时主动拒绝调用**？

实验证明这是当前 Agent 系统的普遍弱点：
- Qwen2.5-7B 经 GSM8K RL 后，R_NTA 从 34.8% 升至 90.2%
- Llama3.1-8B R1-Distill 后，R_DT 达到 100%（每次都捏造工具）

---

## 核心矛盾：Reliability-Capability Trade-off

论文证明：**不可能在增强推理能力的同时不放大工具幻觉**。两者存在根本性耦合：

- 推理 RL 让模型更"自信地生成推理链"
- 这种自信泛化到工具场景，导致模型在工具缺失时仍坚信自己知道该怎么做
- 表征分析显示：推理 RL 在工具相关 query 上产生了远超数学 query 的表征位移

---

## 与现有工作的区别

| 问题 | 已有研究 | 本文发现 |
|------|---------|---------|
| 工具幻觉 | AgentSafetyBench 测量存在性 | 发现推理增强是根因 |
| 自纠正 | Shinn et al. 2023 — 需要外部反馈 | 即使纯推理 RL 也会放大幻觉 |
| CoT 推理 | Budget Forcing 延长推理链 | 不解决幻觉问题 |

---

## 相关概念

- [[ai-agent]] — Agent 的 5C 模型，工具调用是核心能力
- [[mcp]] — MCP 协议标准化工具调用接口
- [[test-time-compute-scaling]] — Test-time scaling 包括 CoT 推理，尚未解决幻觉问题
- [[agent-engineering]] — Agent 工程化实践，工具可靠性是关键挑战
- [[harness-engineering]] — PE→CE→HE 演进，约束与 Agent 能力的关系

---

## 参考

- Yin, C., Sha, Z., Cui, S., Meng, C., & Li, Z. (2025). *The Reasoning Trap: How Enhancing LLM Reasoning Amplifies Tool Hallucination*. arXiv:2510.22977. Accepted to ACL 2026 Main.
- GitHub: https://github.com/albert-y1n/Reasoning_Trap
