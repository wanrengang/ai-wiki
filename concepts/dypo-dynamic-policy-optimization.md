---
title: DYPO — 动态策略优化
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [training, optimization, fine-tuning, reasoning]
sources: [raw/articles/2026-05-14-dypo-dynamic-policy-optimization-tsinghua.md]
confidence: high
---

# DYPO — 动态策略优化

## 概述

DYPO（Dynamic Policy Optimization）来自清华大学深圳国际研究生院、中兴通讯、重庆邮电大学，提出根据 rollout 结果将样本分为三类（Easy/Hard/Mid），动态匹配最优优化路径，实现 SFT 与 RL 的真正协同。

核心洞察：**SFT 负责把模型"扶稳"，RL 负责让模型"继续往外探索"**——问题不是两者能否一起用，而是怎样一起用。

## 核心问题：SFT 与 RL 的协同困境

| 范式 | 偏差 | 方差 | 特点 |
|------|------|------|------|
| SFT | 高 | 低 | 学得快、过程稳、易收敛，但易过拟合示范分布，缺乏泛化 |
| RL | 低 | 高 | 有探索性、能学会推理，但训练波动大、奖励稀疏时易学偏 |

## 三类样本分类机制

### Easy 样本（跳过）
- **判断条件**：一组 rollout 全部成功
- **策略**：直接跳过，减少无效更新
- **原因**：模型已掌握这类问题，继续训练收益有限

### Hard 样本（多教师蒸馏）
- **判断条件**：一组 rollout 全部失败
- **策略**：Multi-Teacher Distillation（多教师蒸馏）
- **方法**：引入多个 teacher，让 student 学习多种合理推理轨迹的共通部分
- **作用**：抵消单一 teacher 的特定偏差，先建立可靠先验，再谈探索

### Mid 样本（GAL 强化学习）
- **判断条件**：一组 rollout 有成功也有失败
- **策略**：RL 优化 + Group Alignment Loss (GAL)
- **GAL 核心**：利用同一组 rollout 中的成败轨迹差异，显式拉向正确轨迹、推离错误轨迹，充当动态方差抑制项

## 实验结果

### Qwen2.5-Math-7B

| 任务类型 | SFT→RL | DYPO | 提升 |
|----------|--------|------|------|
| 5个复杂推理 benchmark 平均 | 47.7 | **52.5** | +4.8 |
| OOD 任务平均 | 48.3 | **61.6** | **+13.3** |

### Qwen3-4B-Base

| 任务类型 | SFT→RL | DYPO |
|----------|--------|------|
| ID 任务平均 | 56.1 | **66.9** |
| OOD 任务平均 | 52.6 | **68.5** |

## 训练稳定性

- **自适应课程学习**：随能力提升，逐步降低对监督信号的依赖——训练从"更靠 teacher 扶着走"→"更依赖策略自己探索"
- **梯度范数**：DYPO 更新轨迹比标准 GRPO 更平滑，有效约束高方差更新

## 关键结论

> DYPO 不是在证明 SFT 和 RL 可以一起用，而是在回答**它们到底应该怎样一起用**。

核心：将优化逻辑前移，**先判断阶段，再匹配路径**。

## 实验边界

- ✅ 数学与逻辑推理场景验证有效
- ❓ 开放式对话、创作类任务效果待观察
- ⚠️ 每个 prompt 需生成 8 条 rollout，算力开销增加

## 相关概念

- [[on-policy-distillation-thunlp]] — 同样是 SFT+RL 协同，但 DYPO 的动态路由思路更系统
- [[fine-tuning-with-trl]] — DYPO 属于 SFT/RL 协同训练方法论范畴
- [[test-time-compute-scaling]] — 数学推理 benchmark（AIME/MATH）是两类方法的共同评测基准
- [[tool-hallucination-reasoning-trap]] — 推理训练的可靠性问题，DYPO 通过多教师蒸馏缓解单教师偏差

## 参考

- 论文：https://arxiv.org/pdf/2604.08926
- GitHub：https://github.com/Tocci-Zhu/DYPO
