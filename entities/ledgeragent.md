---
title: LedgerAgent
created: 2026-06-21
updated: 2026-06-21
type: entity
tags: [model, agent, architecture, research]
sources: [raw/papers/2026-06-21-ledgeragent-structured-state-policy-agent.md]
confidence: high
---

# LedgerAgent

## Overview

LedgerAgent 是亚利桑那州立大学/亚利桑那大学提出的**推理时（inference-time）工具调用 Agent 状态管理与策略合规框架**（arXiv:2606.20529）。核心洞察是：标准 Agent 将任务状态隐式地埋在不断膨胀的 prompt 里，导致两个典型失效模式：

1. **状态陈旧失效**：Agent 正确检索了信息，但在后续决策中使用了过时/缺失/错误重建的状态
2. **策略违规失效**：工具调用语法正确，但违反了依赖当前任务状态的领域策略

LedgerAgent 在不改变模型权重的前提下，通过两个确定性组件解决上述问题。

## Core Components

### 1. Schema-Anchored Ledger（模式锚定账本）

- **数据结构**：类型化字典 `L: P → V`，P 为规范路径（如 `orders.*`、`products.*`、`reservations.*`），V 为工具返回值的集合
- **更新规则**：仅从**成功的只读工具调用**（read tools）吸收返回值；失败工具和写操作不更新状态；写操作后必须发读调用来观察新状态（**observe-not-assume 原则**）
- **渲染方式**：每次模型调用前，将完整账本块注入 prompt，显示在规范路径下（如 `orders.1234 delivered`），Agent 通过查找获取当前状态，而非在原始对话历史中搜索

### 2. Policy Gate（策略门控）

在**环境变更调用**（environment-changing calls，如退款、取消、修改订单）执行前，基于账本状态对其进行 Predicate 校验：

| 结果 | 行为 |
|------|------|
| `allow` | 原样执行 |
| `revise` | 移除调用，返回被违反的 Predicate 给模型 |
| `block` | 阻止调用，拒绝请求的操作 |

Predicate 以代码形式对账本字段编写，而非自然语言策略编译。实验中使用 28 个确定性门控 Predicate：航空 10 个、零售 12 个、电信 6 个、健康 0 个。覆盖所有权检查、实体状态前提条件、参数锚定、退款/支付一致性、循环预防等场景。

## Key Design Insights

- **vs. 训练/微调方法**：Schick et al.、Li et al. 等通过微调/SFT 教模型更可靠地使用工具；RL 奖励成功轨迹；推理时脚手架添加规划/反思。**这些方法保留了"prompt 内隐状态"的本质**，LedgerAgent 改变的是状态表示方式本身
- **observe-not-assume**：账本仅存储从外部系统实际观察到的记录，而非 LLM 摘要或检查清单；写操作后强制重读
- **typed ledger vs. transcript search**：将稳定路径（`orders.1234`）而非原始 JSON 暴露给模型，降低上下文窗口内搜索的不可靠性
- **pass@k 一致性指标**：在更高 k 值下（独立试验多次），标准方法的 pass@k 急剧下降，而 LedgerAgent 仍保持显著优势

## Architecture

```
User Message → Append to History
                    ↓
            Read Tool Returns?
                  No → Step 3
                  Yes → Absorb into Ledger L
                    ↓
            Render Ledger Block C
                    ↓
            Generate Response/Tool Call
                    ↓
            Environment-Changing Call?
                  No → Execute
                  Yes → Policy Gate (check Π over L)
                            ↓
                    allow/revise/block
```

## Experiments

- **Benchmark**：τ²-bench 和 τ-Trait，涵盖航空、零售、电信、医疗四个客户服务领域
- **模型面板**：混合开放权重与闭源模型
- **结果**：LedgerAgent 在大多数领域-模型对上提升 pass@k；高 k 值下增益最大（标准方法一致性最差时）；消融实验确认typed ledger 和 policy-gated action 是主要贡献来源

## Related Concepts

- [[agent-drift-multi-agent-systems]] — 多智能体长期运行行为退化，LedgerAgent 通过显式状态减少状态错误
- [[containment-gap-agentic-framework-security]] — 框架级安全/策略合规问题
- [[tool-hallucination-reasoning-trap]] — 工具调用幻觉与推理陷阱
- [[mem0]] — Agent 记忆层的另一种思路
- [[decoction-experience-memory]] — 经验记忆与上下文策略
- [[test-time-compute-scaling]] — 推理时计算方法，与 LedgerAgent 的推理时组件相关
