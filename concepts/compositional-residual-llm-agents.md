---
title: Compositional Residual — 多组件 LLM Agent 的概率不一致性
created: 2026-05-30
updated: 2026-05-30
type: concept
tags: [agent, multi-agent, research, benchmark, calibration]
sources: [raw/papers/2026-05-30-compositional-incoherence-multi-llm-agents.md]
confidence: high
---

## 定义

**Compositional Residual（合成残差）** 记作 $\varepsilon^*$，定义为组成 quote 到联合相干多面体（joint coherent polytope）的 $L_2$ 距离，是多组件 LLM Agent 系统级相干性的运行时无分布证书。

多组件 LLM Agent 将问题分解给多个专业子 Agent（规划器、检索、算术、概率评估等），每个组件只看到联合问题的一部分。即使每个组件在校准和内部相干性上表现良好，聚合信念仍可能违反基本概率公理——例如研究组件输出 $P(\text{Republican}) = 0.6$ 而预测组件输出 $P(\text{Democrat}) = 0.6$，组装后总概率质量为 1.2，超出合法概率分布。

## 核心机制

### 1. 局部相干，全局不相干（LCGI）故障

每个专业组件在其分配的问题上是局部相干的，但跨组件的逻辑约束对所有组件都不可见。组装后的多组件 quote 可能违反概率公理（如质量不等于 1），引发 Dutch-book 风险。

### 2. 合成残差 $\varepsilon^*$

$$\varepsilon^* = \| \text{composed quote} - \text{joint coherent polytope} \|_2$$

可从系统输出和声明的跨组件耦合约束直接计算，无需知道真实数据分布。

### 3. Product-Structure Dichotomy（定理 3.3）

**关键结论：** 在所有者选择聚合（每个组件拥有一部分联合坐标的子集）下，当且仅当联合多面体分解为局部多面面的笛卡尔积时，组件相干性才能保证系统相干性。在任何更紧密的耦合下，存在局部相干但全局不相干的组件预测。

推论 3.9 将存在性二分法转换为封闭形式的幅值预测（Rayleigh 商形式），可仅从专业面板协方差计算。在四个关系类别中的三个上，Rayleigh 商幅值预测与观察到的残差在 7% 以内匹配。

### 4. 修复：分层 Boyle–Dykstra 投影

层级 Boyle–Dykstra 投影以确定性方式修复组合。配套的 e-process（顺序测试）在任何停止时间控制误报率，提供 anytime-valid 的相干性测试。

### 5. 实证结果

- **数据集：** 1,876 个ensemble cliques，来自 Paleka 和 Polymarket，涵盖四个当代基础模型
- **关键发现：** $\varepsilon^* > 0$ 出现在 33–94% 的 cliques 上（在 partition > negation > disjunction > conjunction 关系类别中）
- **风险量化：** 在比例分配规则下，每个赌注的 regret 增加 +0.115 nats（1,770 个已结算赌注）
- **三种直观缓解措施全部失败或退化：** 检索、分区感知提示、聚合器-LLM

## 与 Agent Drift 的关系

[[agent-drift-multi-agent-systems]] 研究的是多智能体 LLM 系统在**长期运行**中的行为退化（任务成功率下降 42%）；本文研究的是多组件系统在**单次推理**中的概率组合失效机制。两者都是多组件 LLM 系统的系统性故障模式，但角度互补：

| 维度 | Agent Drift | Compositional Residual |
|------|-------------|----------------------|
| 时间尺度 | 长期（73次交互后）| 单次推理 |
| 故障表现 | 任务成功率下降 | Dutch-book 暴露 |
| 根因 | 行为退化/能力侵蚀 | 概率约束缺失 |
| 检测方式 | RSI 指标 | $\varepsilon^*$ 运行时证书 |

## 开放问题

- 是否有实际部署的多组件 Agent 系统真正面临显著 LCGI 风险？
- 分层修复的实际计算开销是否可以接受？
- e-process 的顺序测试是否能在实际部署中实时监控？

## 相关概念

- [[agent-drift-multi-agent-systems]] — 多智能体长期运行退化
- [[ai-sycophancy-analysis]] — LLM 置信度校准问题
- [[harness-engineering]] — 多 Agent 系统的工程约束视角
- [[lcguard-kv-latent-communication]] — 多 Agent 通信安全问题