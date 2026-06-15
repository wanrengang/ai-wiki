---
title: AutoLab
created: 2026-06-05
updated: 2026-06-05
type: concept
tags: [benchmark, agent, long-horizon, optimization, evaluation]
sources: [raw/papers/2026-06-05-autolab-long-horizon-agent-benchmark.md]
confidence: high
---

# AutoLab

## 概述

AutoLab 是超长时闭环优化 Agent 基准测试，由 MIT/港科等机构提出（arXiv 2606.05080）。36 个任务覆盖 4 大领域（系统优化、谜题挑战、模型开发、CUDA kernel 优化），每个任务要求 Agent 在严格 wall-clock 预算内将正确但次优的 baseline 迭代优化至尽可能接近 reference 方案。

## 关键发现

- **claude-opus-4.6** 在所有 4 个子领域均排名第一，Avg@3 = 0.68（第二名仅 0.50）
- **gpt-5.4** 失败原因与原始编程能力无关——提前终止或耗尽预算而无有效输出
- **核心洞察**：最终性能的最强预测因子不是 Agent 首次尝试的质量，而是**持续迭代的坚持度（persistence）**
- 总评估消耗：2544 wall-clock 小时，860 亿 tokens，17 个模型

## 任务设计

- **Instruction + Environment + Verifier + Reference Solution + Wall-clock Budget**
- Environment：容器化沙箱（CPU 或单 GPU），内置 working but unoptimized baseline
- Scoring：锚定相对评分（log-stretch 用于性能指标，linear 用于有界质量指标）。s=0 at baseline，s=0.5 at reference，s→1 接近 practical optimum
- 最小提升门：Agent 必须超过 baseline 才能获得非零分数

## 为什么重要

当前 benchmark 主要评估单轮或短 horizon Agent 轨迹，无法测试小时级持续迭代能力。AlphaEvolve、Karpathy AutoResearch 等最令人印象深刻的经验优化演示都与高度工程化的模型特定 harness 紧耦合。AutoLab 提供标准化评估环境，分离模型真实贡献与工程化 harness 的影响。

## 相关链接

- [[harness-engineering]] — AutoLab CUDA kernel 优化任务与 Harness 工程高度相关
- [[test-time-compute-scaling]] — 持续迭代 vs 单次推理的联系（persistence 作为核心能力）
- [[ai-scientists-autonomous-research]] — AutoLab 解决的正是 AI Scientists 失败的"长 horizon 研究迭代"问题
- [[agent-production-evaluation]] — 评估范式（静态单轮 → 持续迭代）的转变