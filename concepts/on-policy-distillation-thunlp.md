---
title: On-Policy Distillation 蒸馏两大法则（清华 THUNLP）
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [fine-tuning, training, research, optimization]
sources: [raw/articles/2026-05-14-thunlp-opd-on-policy-distillation.md]
confidence: high
---

# On-Policy Distillation 蒸馏两大法则（清华 THUNLP）

## 概述

清华大学 THUNLP 实验室等机构发表的论文（arXiv:2604.13016），系统解剖了 On-Policy Distillation（OPD）失效的根因，并给出了可操作的处方。是 2026 年后训练领域的重要论文。

## 核心发现

### 法则一：思维模式一致性（Thinking-Pattern Consistency）

OPD 成功的前提是 Student 与 Teacher 的思维模式（thinking pattern）初始重叠度足够高。

- 用 Qwen3-1.7B-Base 向两个 Teacher 学习：Qwen3-4B (Non-thinking) vs Qwen3-4B-Base-GRPO（经 GRPO 强化）
- 学生与 Base+GRPO Teacher 的初始 Overlap Ratio 更高，蒸馏效果显著更好
- **早期思维模式错配，后续极难弥补**

### 法则二：高分 ≠ 新知识（Higher Scores ≠ New Knowledge）

更大的 Teacher 并不自动带来更好的蒸馏效果：

- DeepSeek family：Skywork-OR1-Math-7B（经RL）的 gap recovery = **16.9%**；同 pipeline 的 DeepSeek-R1-Distill-7B = **5.3%**
- Qwen family：这个差距达到 **58.6% vs 15.6%**

反向蒸馏实验更极端：让 RL 后的 JustRL-1.5B 向自己的 pre-RL checkpoint 学习（蒸馏回退），效果和向大 5 倍的 R1-Distill-7B 学习几乎一样——都让能力倒退回 RL 前水平。说明 OPD 学的是「思维路径」，不是「分数」。

## 机制：Token 级别发生了什么

- 成功 OPD 中，师生 Top-k Token 重叠率（Overlap Ratio）从 72% 稳步升至 91%+
- 失败的 OPD 中，Overlap Ratio 从头到尾无变化
- **关键发现**：「重叠区域」即是全部——只对 Overlap Token 计算损失，蒸馏性能几乎不打折扣

## 处方

1. **Off-Policy 冷启动**：OPD 前先用 Teacher rollout 做一轮轻量 SFT，拉高初始 Overlap Ratio
2. **Teacher-aligned Prompts**：用与 Teacher 训练分布一致的 prompt，减少分布偏移；需混入部分 OOD prompt 防止熵坍塌

## 局限

- 奖励信号随轨迹深度急剧衰减（15K token 后「从后向前的熵崩塌」）
- OPD 目前难以扩展到长思维链或 agentic 多轮场景
- 全局 reward 有信息，但局部优化几何结构可能平坦

## 相关概念

- [[fine-tuning-with-trl]] — TRL 框架中的蒸馏实践
- [[test-time-compute-scaling]] — RL/推理Scaling与蒸馏的关系
- [[ai-agent]] — OPD 是 [[ai-agent]] 后训练的重要组成部分
- [[harness-engineering]] — OPD 是 PE/RL 流程中的关键环节
