---
title: EurekAgent
created: 2026-06-12
updated: 2026-06-12
type: concept
tags: [agent, autonomous-research, environment-engineering, architecture]
sources: [raw/papers/2026-06-12-eurekagent-tSinghua-zhipu-autonomous-science.md]
confidence: high
---

# EurekAgent

## 概述

EurekAgent 是清华大学 & 智谱 AI 的环境工程（Environment Engineering）Agent 系统，用于指标驱动的自主科学发现。核心洞察：**随着通用 Agent 能力增强，自主科学发现的瓶颈从「规定 Agent 工作流」转移到「设计 Agent 环境」**。

论文：[arXiv 2606.13662](https://arxiv.org/html/2606.13662)，THU-Team-Eureka 开源。

## 核心洞察

传统自主研究系统通过**预设工作流**（prescribing workflows）来指导 Agent：
- AlphaEvolve：维持候选程序种群，用评估反馈指导变异和选择
- AIDE：围绕解树、反馈循环、角色专业化组织探索
- AI Scientist：端到端自动化论文生成、实验、写作

EurekAgent 认为这些方法**编码了过强的研究过程假设**。当通用 CLI Agent（如 Claude Code、Codex）已足够强大时，详细规定工作流反而限制了 Agent 自身能力的发挥。在 ResearchClawBench 上，Claude Code 和 Codex 作为独立通用 Agent，超越了所有评估过的研究专用 Agent 系统。

但仅有任务性能不等于可靠的自主研究者——Agent 可能污染评估、操纵证据、违反程序约束。

**EurekAgent 的答案是：环境工程 > 工作流规定。**

## 四维环境工程

### 1. Permissions Engineering（权限工程）
- Docker 容器隔离运行，每个 Run 有独立 workspace
- 隐藏评估脚本放在 Agent 不可见的区域，通过安全评估服务暴露（Agent 可提交答案并获取分数，但无法查看/修改评估器）
- Controller 持有的文件有 Hook 阻止 Agent 修改
- 同轮并行实现 Session 之间相互隔离（不能查看/复制同轮其他方案）
- GPU 任务：默认拒绝策略，须通过 GPU Helper API 获取，记录锁所有权

### 2. Artifact Engineering（制品工程）
- 文件系统 + Git 历史作为共享长期记忆
- 存储阶段产物：准备摘要、提案清单、假设、解决方案代码、评估反馈、打分提交
- Git 提交跟踪解的演化，每个 commit message 要求说明当前独立解 vs. 前一版本的变更
- 官方分数自动记录并排名

### 3. Budget Engineering（预算工程）
- 双轴控制：wall-clock time + API cost
- 时间：分提案阶段和实现阶段单独限制；Agent 可主动调用时间检查 API，被动时注入警告消息
- 成本：跨 Session 追踪 token 使用，不暴露给 Agent
- 支持中断恢复：每个阶段的状态（Session ID、状态、已用时间、有效预算）持久化，Run 可从最新文件系统状态恢复

### 4. Human-in-the-loop Engineering（人机交互工程）
- Terminal UI：展示每方案进度、原始 Session 输出、输入框通信
- Web Monitor：可视化分数演化、每轮和全局最优方案

## 三阶段循环

```
Prepare → [ Propose_r → { Implement_r,p }_p=1..P ]_r=1..R
```

- **Prepare Stage**：一次性准备，运行配置测试、验证依赖、安装
- **Propose Stage**：每轮开始，提案 Agent 读取任务输入+准备摘要+历史最优解，生成 P 个候选假设清单
- **Implement Stage**：fan-out，每假设一个独立并行实现 Session，独立 workspace，独立评分提交

## 实验结果

| 任务 | EurekAgent | Prev. Best AI | 说明 |
|------|-----------|---------------|------|
| 26-Circle Packing | 2.635999 | 2.635986 | 新 SOTA，API 成本 <$11 |
| Erdős Min Overlap | 0.380870 | 0.380876 | 新 SOTA |
| 1st Autocorr. Ineq. | 1.502861 | 1.502863 | 新 SOTA |
| TriMul Kernel | 2005.03µs | 2247.78µs | 新 SOTA |
| MLE-Bench 子集 | 85.71% | 71.43% | 排名第一 |

配置：Claude Code（CLI Agent）+ GLM-5.1（基座 LLM）+ Web Search Prime MCP + Playwright MCP。

## 关键发现

1. **通用 Agent + 环境约束 > 专用工作流 Agent**：ResearchClawBench 40 个研究任务、10 个领域，Claude Code/Codex 独立使用优于所有研究专用系统
2. **26 圆填充 <$11 发现新 SOTA**：极低 API 成本证明环境工程的有效性
3. **环境工程四维度是通用的**：permissions/artifact/budget/HitL 可以迁移到其他自主研究系统

## 相关概念

- [[harness-engineering]]：约束越多 Agent 干得越好，PE→CE→HE 演进，EurekAgent 是 HE（Harmonization Engineering）的具体实现
- [[ai-scientists-autonomous-research]]：AI Scientists 自主研究尝试，EurekAgent 解决了「奖励黑客」和「可观测性失败」问题
- [[mlevolve-self-evolving-mle-agent]]：MLEvolve 自我演化 MLE Agent，EurekAgent 的环境工程思路与之互补
- [[siga-self-evolving-coding-agent-adapters]]：SIGA 自进化编码 Agent 适配器，同样关注 Agent 自我改进
- [[agent-memory-systems-characterization]]：Agent 记忆系统表征研究，EurekAgent 的 Artifact Engineering 可类比为外部化记忆
- [[hermes-cron]]：Hermes 定时任务系统，支持流水线与监控，类比 EurekAgent 的三阶段循环编排