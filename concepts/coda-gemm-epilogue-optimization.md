---
title: CODA — GEMM-Epilogue 重参数化：LLM 也能写光速内核
created: 2026-05-25
updated: 2026-05-25
type: concept
tags: [inference, optimization, architecture, training]
sources: [raw/articles/2026-05-25-coda-rewriting-transformer-blocks.md]
confidence: high
---

# CODA — GEMM-Epilogue 重参数化：LLM 也能写光速内核

## 概述

CODA（CODa: Rewriting Transformer Blocks as GEMM-Epilogue Programs）是由 MIT、普林斯顿、Together AI 和 Meta 研究者联合发布的论文（arXiv 2605.19269），核心思想是将 Transformer 中的非矩阵乘法操作（归一化、激活函数、残差加法等）通过代数重参数化，塞入 GPU GEMM 内核的「尾声」（epilogue）阶段执行，从而消除中间结果的显存读写，获得显著的端到端加速。

论文地址：https://arxiv.org/abs/2605.19269
代码地址：https://github.com/HanGuo97/coda-kernels

## 核心洞察

### 内存带宽瓶颈

在大模型训练中，矩阵乘法（GEMM）和注意力计算确实是主要算力消耗，但还有一批「小算子」——RMSNorm、SwiGLU、RoPE、残差加法——它们单次计算量不大，却频繁地把大型中间张量从显存搬进搬出。随着 FP8/FP4 低精度格式让矩阵计算越来越快，这些「搬运」操作的相对成本反而在上升。^[raw/articles/2026-05-25-coda-rewriting-transformer-blocks.md]

### 尾声（Epilogue）的黄金窗口

GPU 上的高性能 GEMM 内核分为两部分：**主循环（mainloop）** 负责核心矩阵分块乘加计算，**尾声（epilogue）** 在结果写回显存之前做收尾处理（如加偏置、类型转换）。当矩阵乘法的输出还「活在」片上寄存器里、尚未落地显存时，这是一个短暂的黄金窗口——若能在此时多做一些计算，就可以省掉一次显存写入再读出的往返。

CODA 的核心洞察是：Transformer 中的内存密集型操作（如 RMSNorm）可以被代数地重新参数化，塞进这个「尾声」窗口执行。

以 GEMM-RMSNorm-GEMM 模式为例：传统做法是三个独立算子串行执行，中间结果两次落地显存。CODA 团队发现，RMSNorm 的行缩放因子 r（每行共享标量）与后面的矩阵乘法满足交换律——可以把 r 的应用从「第二个 GEMM 之前」推迟到「第二个 GEMM 的尾声」。推迟后第一个 GEMM 的尾声只需计算局部的「分块均方根」（partial RMS），完整的 RMSNorm 计算消失了。

类似的重新参数化对 SwiGLU、RoPE、交叉熵损失等同样适用，甚至对反向传播也成立（定理：只要前向尾声是「分块局部」的，反向传播自动继承相同结构）。

## 编程抽象：五类原语

CODA 不是具体的融合内核，而是一套基于 NVIDIA CUTLASS Python DSL（CuTeDSL）的编程抽象。它固定住专家优化的 GEMM 主循环，在尾声位置暴露五类可组合的基本原语：

| 原语类型 | 功能 |
|----------|------|
| 逐元素变换 | residual 加法、激活函数、RoPE |
| 向量加载与存储 | 广播 RMSNorm 权重 |
| 矩阵分块加载与存储 | 保存中间激活供反向传播 |
| 分块规约 | 局部均方根、分块 log-sum-exp |
| 有状态变换 | 在线归一化的 max 和 sum-exp 统计 |

## 关键实验结果

### LLM 生成 vs 人工手写

论文同时评估了 CODA (LLM) 和 CODA (Human) 两种实现：
- **CODA (LLM)**：由 Claude Code 生成，研究者提供原语说明、示例和实现日志，AI 完成主体代码，人工轻度监督
- **CODA (Human)**：人工程序员独立编写，使用同样的重参数化思路

**结论：LLM 生成的内核在大多数基准上与人工手写版本不相上下，个别配置甚至略有超越。** 这在 GPU 内核优化这个历来门槛极高的领域，是一个颇为罕见的结论。

### 性能提升数据

| 操作 | 加速倍数 |
|------|---------|
| GEMM-Residual-PartialRMS-GEMM 反向传播 | **1.6×–1.8×** |
| SwiGLU 反向传播 | **1.4×–1.6×** |
| 完整 Transformer 层前向（端到端） | **5%–20%** |

在较大模型尺寸（70B 规模隐藏维度）下效果更为显著。数值精度与 PyTorch 参考实现相当，部分配置误差更小（得益于 GEMM 主循环更高精度的累加器）。

### 覆盖范围

标准 Transformer（如 LLaMA 架构）前向和反向传播中，**除注意力和词嵌入之外的几乎全部计算**：RMSNorm、残差加法、SwiGLU、RoPE、交叉熵损失。

**当前限制**：仅支持单 GPU，不涉及分布式训练；主要针对标准 Transformer 架构。

## 技术脉络

CODA 并非孤立的工作。它是同一类思想的具体实现——在 GPU 上，真正的优化空间往往不在「算什么」，而在「怎么搬」：

- **FlashAttention** 让注意力计算「住进」了片上内存
- **CODA** 试图让归一化和激活函数也「住进去」
- **Triton** 降低了写自定义内核的门槛
- **ThunderKittens、TileLang** 等进一步在不同层次上探索这一空间

## 相关概念

[[triattention-kv-compression]] — 同样来自 MIT/NVIDIA/浙大的 GPU 内存优化工作，KV 选择性压缩
[[gated-deltanet-2]] — NVIDIA 的另一个线性注意力改进工作
[[test-time-compute-scaling]] — 推理优化相关方向
[[lilian-weng-why-we-think]] — Lilian Weng 对推理机制的深度解析