---
title: Gated DeltaNet-2
created: 2026-05-24
updated: 2026-05-24
type: concept
tags: [model, architecture, inference, optimization]
sources: [raw/articles/2026-05-24-gated-deltanet-2-nvidia-linear-attention.md]
confidence: high
---

# Gated DeltaNet-2

## 概述

Gated DeltaNet-2 是 NVIDIA 提出的线性注意力新架构，将线性注意力中的「擦除」和「写入」操作解耦为独立的通道级门控。核心创新是用**通道级擦除门**和**通道级写入门**替代了以往方法的单一标量门，从根本上解决了固定状态循环中多 association 干扰的核心问题。

论文：[arXiv:2605.22791v1](https://arxiv.org/html/2605.22791v1)，代码：[github.com/NVlabs/GatedDeltaNet-2](https://github.com/NVlabs/GatedDeltaNet-2)

## 背景问题

线性注意力用固定大小的循环状态替代 softmax 注意力的无限缓存，实现线性时间复杂度和常数内存解码。但压缩后的 KV 内存面临核心矛盾：

- **状态有限**：长上下文强制许多 association 共享同一有限空间
- **精确检索困难**：每次外积都加入状态、但没有任何内容被移除（除非被覆盖）

### 历代解法

| 方法 | 擦除 | 写入 | 衰减 |
|------|------|------|------|
| Mamba-2 | 标量 | 满 | 标量 |
| Gated DeltaNet | 标量 | 标量 | 标量 |
| KDA (Kimi Delta Attention) | 标量 | 标量 | **通道级** |
| **Gated DeltaNet-2** | **通道级** | **通道级** | **通道级** |

KDA 将衰减变成通道级，但 active edit 的 β_t 仍是标量——同时控制「从读取方向擦除多少旧内容」和「向值方向写入多少新内容」，这是建模限制而非 delta 规则的内在要求。

## 核心创新：Gated Delta Rule-2

### 解耦擦除与写入

引入两个独立门控：
- **擦除门** b_t ∈ [0,1]^{d_k}（key 轴）：决定从读取方向擦除哪些坐标
- **写入门** w_t ∈ [0,1]^{d_v}（value 轴）：决定向值方向提交哪些坐标

状态更新分三步：
1. **衰减**：S̄_t = D_t × S_{t-1}（D_t 是通道级衰减对角矩阵）
2. **读取**：r_t = S̄_t^T × e_t（e_t = b_t ⊙ k_t，通道级门控的读取方向）
3. **写入**：S_t = S̄_t + k_t × (z_t - r_t)^T（z_t = w_t ⊙ v_t，通道级门控的写入目标）

### 快速权重更新视角

Gated Delta Rule-2 是局部在线优化问题的解析解：

```
L_t(S) = ‖S - S̄_t‖_F^2 - 2⟨S^T×k_t, z_t - S̄_t^T×e_t⟩
```

最小化即得上式。直观理解：保持与衰减内存接近，同时通过 associative edit 修正——残差是比较 gated 写入目标与从 S̄_t 沿 e_t 读出的内容。

### 与先前方法的关系

- 当 b_t = β_t·1 且 w_t = β_t·1 时 → **精确恢复 KDA**
- 当上述条件 + 衰减也是标量时 → **精确恢复 Gated DeltaNet**
- 保留了 KDA 的通道级衰减和高效的 WY 分块算法

## 实验结果

- **规模**：1.3B 参数，100B FineWeb-Edu tokens
- **对比**：Mamba-2 / Gated DeltaNet / KDA / Mamba-3 各变体
- **全面领先**：语言建模、常识推理、检索任务均最强
- **最大优势场景**：长上下文 RULER needle-in-a-haystack 基准
  - especially 多 key 检索（固定大小状态分离竞争 association）
  - recurrent 和 hybrid 两种设置下均强

## 与本 Wiki 其他页面的关联

- [[test-time-compute-scaling]]：长上下文推理优化是测试时计算扩展的重要方向
- [[triattention-kv-compression]]：KV 压缩与线性注意力解决同一工程问题（内存），但路径不同
- [[memory-processing-pipeline-llm-inference]]：记忆管道也关注 LLM 推理效率
- [[inference-optimization]]：Gated DeltaNet-2 属于推理优化类别的架构创新

## 技术细节

- **训练效率**：通过将累积通道级衰减吸收进秩一擦除因子，保留与高效 delta-rule 内核相同的分块结构
- **Gate 参数化**：两个门由 token 表示的独立投影生成；日志衰减跟随 Gated DeltaNet 参数化
- **负特征值变体**：支持将擦除门缩放到 [0,2]^{d_k} 以处理负特征值情况