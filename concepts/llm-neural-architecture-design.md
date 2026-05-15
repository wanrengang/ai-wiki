---
title: LLM 神经网络架构设计
created: 2026-05-13
updated: 2026-05-13
type: concept
tags: [model, architecture, training, fine-tuning]
sources: [raw/articles/2026-05-13-llm-neural-architecture-design-2601.02997.md]
confidence: high
---

## 概述

LLM 不仅能生成代码，还能**自主设计新颖的神经网络架构**。这是 2026 年 Waleed Khalid 等人在 arXiv:2601.02997 中发表的论文，展示了 LLM 如何从"记忆"过渡到"创造"。

## 核心问题

> 能否对 LLM 进行迭代微调，使其成为一个可靠生成有效、高性能、结构新颖的神经网络架构的生成器？

## 方法：闭环神经架构合成

系统经过 22 个循环周期，每个周期包含：

1. **Generate** → LLM 采样 PyTorch 架构代码
2. **Validate** → 语法/编译检查
3. **Evaluate** → 单轮 CIFAR-10 准确率
4. **Filter** → MinHash-Jaccard 新颖性检测
5. **Fine-tune** → LoRA 适应（基于成功的生成）

### 关键设计

- **Base Model:** DeepSeek-Coder-7B-Instruct-v1.5（LoRA 微调）
- **API Contract:** 固定的 `Net(nn.Module)` 类接口（`__init__`, `forward`, `train_setup`, `learn`）
- **约束：** 最大 500K 参数、仅标准操作（conv/pool/norm/activations）、无预训练权重
- **新颖性过滤：** Token-level shingling (k=10) + MinHash (256 permutations) + LSH (threshold 0.85)

## 核心结果

| 指标 | 循环 1 | 循环 10 | 循环 18 | 循环 22 |
|------|--------|---------|---------|---------|
| 有效生成率 | 44.0% | 53.8% | 59.1% | 41.8% |
| 最佳准确率 | 47.78% | 55.48% | **63.98%** | 57.62% |
| 平均准确率 | 28.06% | 37.70% | **50.99%** | 49.48% |
| ≥40% 准确率 | 2.04% | 38.04% | **96.81%** | 92.86% |

**关键洞察：** 经过 22 个循环，LLM 生成了 **455 个全新架构**，从随机代码生成器进化为可靠的**架构先验**。

## 技术细节

- **LoRA 配置:** rank=32, α=32, dropout=0.05, 目标模块 q/k/v/o + MLP
- **微调超参:** lr=1×10⁻⁵, 5 epochs/cycle, bfloat16, 8-bit paged AdamW
- **生成参数:** temperature=0.20, top-k=50, top-p=0.9

## 核心发现

> *"LLMs can internalize empirical, non-textual rewards to transcend their training data"*

LLM 能够将**非文本的经验奖励**内化，超越其训练数据——这是从记忆到创造的关键跃迁。

## 局限性

1. 仅在 CIFAR-10 上验证，泛化性未知
2. 单轮准确率作为代理指标，可能偏好"快学"型架构
3. 固定阈值选择策略可能导致过拟合
4. 文本级新颖性不等于计算图级别的语义等价

## 相关概念

- [[test-time-compute-scaling]] — 两者都探索 LLM 的推理时扩展能力
- [[fine-tuning-with-trl]] — LoRA 微调技术细节
- [[agent-engineering]] — 闭环反馈机制是 Agent 工程化的典型范式
- [[ai-scientists-autonomous-research]] — 与 AI Scientists 类似，都展示 LLM 超越文本的自主探索能力
