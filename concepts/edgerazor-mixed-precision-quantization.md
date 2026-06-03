---
title: EdgeRazor 混合精度量化感知蒸馏
created: 2026-05-26
updated: 2026-05-26
type: concept
tags: [model, optimization, inference, training, fine-tuning]
sources: [raw/articles/2026-05-26-edgerazor-mixed-precision-quantization.md]
confidence: high
---

# EdgeRazor 混合精度量化感知蒸馏

## 概述

EdgeRazor 是由南京大学 LAMDA 和微软 AI 联合推出的开源工具库，解决大语言模型端侧部署的核心痛点——极低比特量化的"不可能三角"：后训练量化（PTQ）精度崩塌、量化感知训练（QAT）算力成本极高、量化感知蒸馏（QAD）缺乏灵活性。

核心成绩：1.58-bit Qwen3-0.6B 实现 **15.16× 解码加速**，压缩至 190MB，训练 tokens 消耗降低 75%-90%（最低 3.1B vs ParetoQ 的 30B）。

## 核心架构：三大模块

### SQMP：结构量化与混合精度（Structural Quantization with Mixed Precision）
打破传统量化统一位宽的设定，支持在输入通道维度进行 4-bit 和 1.58-bit 细粒度混合（例如实现 1.88-bit 或 2.79-bit 平均位宽）。交错的高精度行作为"缓冲区"，吸收激活异常值带来的量化误差。

### LAFD：层自适应特征蒸馏（Layer-Adaptive Feature Distillation）
通过计算教师模型相邻层的余弦相似度，自适应地找出对特征转换最关键的 Top-k 层进行重点蒸馏。避免盲目依赖人工经验，有效阻止量化误差在层间放大。

### EAKLD：熵感知的 KL 散度（Entropy-Aware KL Divergence）
纯粹依靠教师模型输出分布的熵动态调节前向/反向 KL 散度比例。完美兼容人工标注数据和模型合成数据，实现训练数据配比自由。

## 核心性能数据

| 指标 | 数值 |
|------|------|
| 1.58-bit 解码加速 | **15.16×**（vs 16-bit 基座）|
| 端到端响应速度 | 10-11× 提升 |
| 磁盘占用（1.58-bit Qwen3-0.6B） | 190MB（1/5.8）|
| 峰值运行内存 | 降至 1/2.9 |
| 量化参数覆盖率 | **99.99%**（传统方法 73.89%）|
| 压缩比（1.58-bit） | **7.03×**（传统方法 2.94×）|
| 训练成本（MobileLLM-350M） | 3.1B tokens（vs ParetoQ 30B）|

## 技术意义

1. **打通全链路**：低成本量化 + 轻量化训练 + 极低成本部署
2. **百兆级端侧部署**：扫清大模型向智能手机、IoT 等内存受限设备迁移的物理障碍
3. **算力降维**：单机 8 卡即可训练（vs ParetoQ 需 16 卡）

## 相关概念

- [[gated-deltanet-2]]：NVIDIA 线性注意力架构，同属端侧优化方向
- [[triattention-kv-compression]]：KV 压缩方向，另一内存优化路径
- [[memory-processing-pipeline-llm-inference]]：LLM 推理内存优化全景
- [[test-time-compute-scaling]]：推理优化总览
- [[lenvm]]：token 级长度控制，与量化正交但同属轻量化方向

## 参考

- 论文：arXiv [2605.04062](https://arxiv.org/abs/2605.04062) v2
- GitHub：https://github.com/zhangsq-nju/EdgeRazor
- Hugging Face：https://huggingface.co/collections/zhangsq-nju/edgerazor-nbit