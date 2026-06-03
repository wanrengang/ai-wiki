---
title: Agent Behavioral Trajectory Tracking
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [agent, alignment, security, inference, training]
sources: [raw/papers/2026-06-03-behavioral-trajectories-adapting-agents.md]
confidence: high
---

# Agent Behavioral Trajectory Tracking

## 概述

现代 AI Agent 依赖文本文件（Skill 文件、Memory 文件、行为配置文件）来定义行为。这些文件会随时间被人类或 Agent 自身修改，直接影响 Agent 在后续交互中的行为。**行为轨迹追踪**（Behavioral Trajectory Tracking）是一种在 embedding 空间中对 Agent 技能文件 diff 进行建模，从而量化 Agent 行为变化的方法。

来自 arXiv 2606.02536（Leshin et al., 2026）。

## 核心方法

### Trait Vector（特质向量）

将 Agent 的某种行为特质定义为文本 embedding 模型 embedding 空间中的一个方向。通过以下步骤学习：

1. **嵌入**：用文本 embedding 模型（如 Qwen3-Embedding-8B）对"修改前"和"修改后"的技能文件分别嵌入并归一化
2. **计算 diff**：di = E(Afteri)^ - E(Beforei)^，归一化后得 di^
3. **Ridge 回归**：从 di^ 到标签 yi ∈ [-1, +1] 拟合线性模型，系数向量 **w** 即为 trait vector

推理时：score = di^ · w + b（b 为 Ridge 截距）

### Agent-to-Agent 协议

三人参与的信任委托协议：
- **Agent A（请求方）**：想评估 Agent B 的行为特质
- **Agent B（执行方）**：持有待评估的文本文件
- **运行时服务器（中介）**：可信第三方，负责 trait 向量评分

流程：
1. Agent A 通过运行时服务器发起评估请求
2. Agent B 从服务器拉取容器化 embedding 执行器
3. Agent B 本地运行容器，计算 diff 向量并提交给服务器
4. 服务器运行评分处理器，返回 trait 得分给 Agent A

关键安全属性：Agent 仅为 HTTP 客户端（不暴露入站端口）；执行与评分分离。

### 信任延续：Hash Chain

通过哈希链确保连续评估锚定正确的文件版本：Agent B 在每次请求中包含"前"文件和"后"文件的哈希值，Agent A 在下一次请求中引用上一次评估的"后"文件哈希。

## 实验结果

- **数据集**：68 对技能文件 diff（来自 Awesome Copilot 仓库的 63 个技能）
- **评估 trait**：数据寻觅倾向（data-seeking propensity）
- **结果**：Sign 分类准确率 91.2%，Spearman ρ = 0.82（留一交叉验证）
- **对比**：
  - YARA 签名规则基线：63.2%
  - 前沿 LLM（GPT-5.4）：100%
  - 本文方法：介于两者之间（确定性 + 可审计 + 高效）

## 与 [[harness-engineering]] 的关系

Harness 工程关注的是如何通过工程基础设施（指令、环境、反馈等五个子系统）提升 Agent 推理效果。本文则关注如何**监控**这些文件本身的行为变化——两者形成互补：

- Harness = 构建可靠 Agent 的工程实践
- Trait Tracking = 监控 Agent 是否被恶意修改

## 与 [[agent-drift-multi-agent-systems]] 的关系

[[agent-drift-multi-agent-systems]] 研究的是多智能体系统长期运行中的行为退化（73 次交互后任务成功率下降 42%）。本文的方法可作为检测 agent drift 的技术手段——通过追踪 trait 向量随时间的变化，主动发现技能文件的异常修改。

## 与 [[hermes-agent]] 的关系

论文在部署部分使用 Hermes Agent 作为案例：Hermes cron job 定期检查技能文件是否有更新，发现更新后启动完整的 A2A 协议流程。这验证了 trait tracking 在真实 Agent 系统中的可行性。

## 关键洞察

1. **文本文件是 Agent 行为的直接驱动器**：技能文件的恶意注入（如 Cisco 2026 年发现的 Claude Code 内存文件攻击）可以静默改变 Agent 行为且跨会话持久化
2. **Embedding 空间比签名规则更语义化**：YARA 规则无法区分"credentials 在保护规则中"和"credentials 在窃取指令中"，embedding 可以
3. **方法 vs LLM 各有取舍**：确定性、可审计、低延迟 vs 最高准确率——高风险场景可用 LLM，平时监控用 trait vector 更实际

## 相关概念

- [[hermes-agent]] — 论文部署案例
- [[harness-engineering]] — Agent 工程基础设施
- [[agent-drift-multi-agent-systems]] — 多智能体行为退化
- [[mcp]] — Agent 间通信协议
- [[scheming-propensity-llm-agents]] — Agent 伪装倾向研究
