---
title: Agent生产评估体系
created: 2026-06-02
updated: 2026-06-25
type: concept
tags: [agent, engineering, evaluation, production, benchmark]
sources: [raw/articles/2026-06-02-agent-production-evaluation.md]
confidence: high
---

# Agent生产评估体系

## 概述

2026 年以来，Agent 的行业讨论正在从跑分、demo 和工具调用能力转向企业部署。代码仓库、内部数据、客服和运维流程把 Agent 带进真实账号、工具权限和人工审查链路，标准任务完成率难以覆盖权限、链路、成本、审查和事故追责问题。

## Agent benchmark 的定位变化

过去一年，Agent 在公开演示和 benchmark 中已经不再只是回答问题。网页浏览、代码修改和软件环境操作等连续任务，开始成为外界衡量 Agent 能力的主要方式。

### 能证明什么，漏掉了什么

| 跑分能证明 | 跑分漏掉 |
|-----------|---------|
| 单次答案质量 | 上线后的发布风险 |
| 工具调用能力 | 运行中的链路观测 |
| 任务完成率 | 审查/修复/切换工具成本 |

高分 Agent 与可部署生产力之间的差距来自三个方面：
- **权限问题**：真实账号和工具权限链路
- **链路问题**：执行过程的可观测性
- **成本问题**：验证和修复的人力时间成本

## 生产评估三层次

### ① 上线前：行为测试

对应指标：覆盖率、测试规格、发布门禁

Benchmark 跑分高不代表系统能通过行为测试覆盖。企业需要把单元测试、集成测试扩展到 Agent 行为层面，验证指令覆盖率、工具调用路径、边界条件处理。

### ② 运行中：链路观测

对应指标：运行轨迹、工具调用、模型路由、延迟、token 消耗、成本、服务容量

Galileo、Datadog 和 Harness.io 的报告都指出，运行中缺少数器会导致故障难以复现和定位。每一次模型路由决策、工具调用、容量错误都必须被记录。

### ③ 事故后：组织指标

对应指标：审查时间、修复时间、工具切换成本、开发者信任

AI 进入工程组织后，审查、修复和切换工具的时间会改变真实交付成本。AI 编程带来的成本不只是 token 消耗，还包括人工审查和修复的人周。

## Harness 报告的核心发现

Harness.io 的报告给出了三点结论：

1. **执行框架需要生产评估体系配合** — 即便执行框架能提升运行稳定性，还需要判断 Agent 行为是否被测试、执行链路是否被记录、验证成本是否被计入
2. **三类信号必须被捕捉** — 上线前行为有没有被测试覆盖，运行中链路有没有被观测，AI 引入后的验证成本有没有被组织指标捕捉
3. **模型能力决定上限，Harness 决定你能用到上限的几成** — 没有 Harness，Opus 4.5 跑出的代码连编译都过不去；有了 Harness，小一档的模型也能稳定交付

## 补充来源（2026-05-24 机器之心）

Springer 对 **15 个主流 Agent benchmark** 的系统综述补充了关键发现：
- **零个** benchmark 将安全性或安全防护纳入评分
- **零个** benchmark 将成本效率纳入主要评估协议
- 15 个中 **13 个** 主要依赖二元成功指标（判断任务是否完成），不说明执行过程是否稳定、可控、可复现

来源：raw/articles/2026-06-25-agent-production-eval-wechat.md

## 相关链接

- [[harness-engineering]] — Harness 工程五子系统
- [[agent-drift-multi-agent-systems]] — 多智能体长期运行退化
- [[test-time-compute-scaling]] — 推理时计算扩展
- [[mcp]] — 模型上下文协议
- [[superpowers]] — AI 编程增强系统
- [[verkor-design-conductor]] — Design Conductor 是首个将 Agent 投入完整芯片生产流程的案例
- [[autolab-long-horizon-agent-benchmark]] — AutoLab 揭示持续迭代坚持度比首次尝试质量更能预测最终性能