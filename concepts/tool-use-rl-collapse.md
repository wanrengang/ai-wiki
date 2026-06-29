---
title: Tool-Use RL Collapse
created: 2026-06-26
updated: 2026-06-26
type: concept
tags: [agent, reinforcement-learning, training, tool-use, architecture]
sources: [raw/papers/2026-06-26-tool-rl-collapse.md]
confidence: high
---

# Tool-Use RL Collapse

## 概述

**核心发现：** 多步工具调用任务的 RL 训练会导致**结构性崩溃（Structural Collapse）**——不是能力退化，而是控制 token 序列的结构性损坏。底层任务能力实际完好，只是被特定格式所掩盖。

来自中科院自动化所的研究（arXiv:2606.26027），系统研究了 Agentic RL 在多步工具调用任务中的失效机制，并提出多种监督信号修复策略。

## 核心现象

### 灾难性崩溃（Catastrophic Collapse）

- 直接对 Qwen2.5-1.5B-Instruct 做 RL → 奖励骤降、KL 散度持续飙升
- Qwen3-1.7B 崩溃更温和，但纯 RL 收益有限且会随时间退化
- SFT 预训练后再 RL → 训练稳定，Qwen2.5-1.5B 曲线平滑

### 崩溃的真正原因

**不是能力退化，而是控制 token 动态失衡：**

RL 过度放大特殊控制 token（如 `<tool_call>`、`<|im_end|>`），导致它们被错误组合。模型从有效的工具调用轨迹漂移到终止模式。

四种结构状态：
- **Healthy Tool Call**：严格遵循 schema 的格式正确工具调用
- **Healthy Response**：不含工具相关伪迹的自然语言响应
- **Text Pollution**：不完整工具标签/错位控制 token 开始出现（中间态）
- **Collapsed**：严重退化，输出完全无效或错误

## 监督信号系统研究

研究团队系统探索了 6 种监督配置（两类模型家族），分为**同步**和**交错**两类训练范式：

| 方法 | 说明 | 效果 |
|------|------|------|
| SFT→RL（同步） | 标准两阶段 | 提供稳定初始化 |
| Off-policy 监督 | 将专家轨迹混入 RL 回放缓冲区 | 部分稳定 |
| Hint-based 引导 | 生成前加入正确提示，优化时移除 | 有潜力 |
| 错误轨迹监督 | 在失败样本上训练识别错误 | 有限 |
| 交错 SFT+RL | SFT 和 RL 交替训练 | **最稳定，但 OOD 性能退化** |
| Process Reflection Supervision | 从中间推理步骤提取反思作为文本监督信号 | 稳定性和最终性能均提升 |

## 关键结论

1. **Agentic RL 失败是结构性崩溃问题，而非能力限制** — 核心问题在于 token 级执行模式的结构完整性，而非推理能力损失
2. **交错训练一致提升稳定性和泛化性** — 同步方法容易遭遇分布不匹配
3. **监督信号的核心作用是控制结构性行为** — 超出标量奖励能单独实现的范围
4. **SFT 预训练是必要的稳定化步骤** — 为 RL 提供高质量初始化，防止崩溃

## 与现有工作的关系

- 与 [[harness-engineering]] 正交：HE 关注的是 harness 层面的工程约束，而本文揭示的是 RL 算法层面的内在失效机制
- 与 [[test-time-compute-scaling]] 对比：test-time scaling 在工具调用任务中效果有限（本文发现）
- 与 [[toolbench-x]] 互补：ToolBench-X 研究工具环境不可靠性下的 agent 鲁棒性；本文研究 RL 训练过程本身的崩溃机制

## 相关链接

- [[harness-engineering]] — Harness 工程学（PE→CE→HE 演进）
- [[test-time-compute-scaling]] — 推理时计算扩展
- [[toolbench-x]] — ToolBench-X 工具可靠性基准
- [[agent-drift-multi-agent-systems]] — 多智能体系统长期行为退化
- [[skillweaver-compositional-skill-routing]] — 工具组合路由研究
- [[hypertool]] — 超越逐步工具调用
