---
title: DreamReasoner-8B
created: 2026-06-18
updated: 2026-06-18
type: concept
tags: [model, architecture, reasoning, inference, diffusion]
sources: [raw/papers/2026-06-18-dreamreasoner-8b-block-diffusion-reasoning.md]
confidence: high
---

# DreamReasoner-8B

## 概述

DreamReasoner-8B 是由香港大学和北京大学联合开发的**首个开源块扩散（Block Diffusion）推理模型**（arXiv:2606.19257，2026-06-17）。

核心问题：自回归（AR）推理模型虽然链式思维（CoT）效果好，但解码时必须串行生成 token，延迟高、算力浪费。块扩散通过**块内并行、块间串行**的方式实现"既快又好"的推理，但此前训练 block size 与推理 block size 的粒度不匹配问题导致大 block 时效果急剧下降。

## 核心贡献

**1. 发现 Block Size 不匹配问题（Block Size Mismatch）**
- 训练时用小 block size → 推理效果正常
- 训练时用大 block size → 推理效果急剧下降
- 大 block 推理时，模型无法正确"重建"细粒度推理步骤

**2. 提出块大小课程学习（Block-Size Curriculum Learning）**
- 训练初期：用细粒度（小 block）学习精确推理
- 训练中后期：逐步过渡到粗粒度（大 block）以获得并行加速
- 核心思想：课程学习让模型从易到难掌握推理的"跳步"能力
- 最终实现在任意推理 block size 下都能保持良好推理质量

**3. 在数学和代码推理基准上与 Qwen3-8B 持平**

## 技术细节

| 维度 | 内容 |
|------|------|
| 模型 | DreamReasoner-8B-Base（基础）+ 对齐版本 |
| 机构 | 香港大学、北京大学 |
| 方法 | 块扩散 + 块大小课程学习 |
| 推理加速 | 块内并行（block-wise parallel denoising）+ 块间自回归 |
| 开源 | https://github.com/DreamLM/DreamReasoner |
| 发布日期 | 2026-06-17 |

## 核心洞察

块扩散推理的本质矛盾：
- **Block size 小** → 接近逐 token 生成，推理质量高，但并行度低
- **Block size 大** → 块内并行度高、延迟低，但推理质量崩塌

课程学习解决的是：从细粒度到粗粒度的**迁移**问题，而非简单扩大 block。

## 相关概念

- [[test-time-compute-scaling]] — CoT 推理的时间 scaling 框架，DreamReasoner 是其中的非 AR 架构替代方案
- [[lilian-weng-why-we-think]] — Lilian Weng 对推理机制的系统解析，CoT 是核心背景
- [[reasoning-in-memory-rim]] — ACL 2026 的固定记忆推理，另一种并行推理思路

## 与 AR 推理模型对比

| | AR 推理（Qwen3/R1） | 块扩散推理（DreamReasoner） |
|---|---|---|
| 并行度 | 低（逐 token） | 高（块内并行） |
| 推理质量 | 高 | 与 Qwen3-8B 持平 |
| 解码延迟 | 高 | 可灵活调节 |
| 训练难度 | 相对简单 | 需要课程学习解决 mismatch |
| 可解释性 | CoT 链清晰 | 块内双向去噪，过程较黑盒 |
