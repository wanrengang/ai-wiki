---
title: CODA (arXiv 2605.19269)
created: 2026-05-25
updated: 2026-05-25
type: entity
tags: [architecture, training, optimization, research]
sources: [raw/articles/2026-05-25-coda-rewriting-transformer-blocks.md]
confidence: high
---

# CODA (arXiv 2605.19269)

## 基本信息

- **论文标题**：CODA: Rewriting Transformer Blocks as GEMM-Epilogue Programs
- **arXiv**：https://arxiv.org/abs/2605.19269
- **代码**：https://github.com/HanGuo97/coda-kernels
- **发布日期**：2026年5月22日
- **作者机构**：MIT、普林斯顿、Together AI、Meta

## 核心贡献

将 Transformer 中的非矩阵乘法操作（RMSNorm、SwiGLU、RoPE、交叉熵等）通过代数重参数化，塞入 GPU GEMM 内核的「尾声」（epilogue）阶段执行，从而消除中间结果的显存读写。

## 关键数字

- 反向传播加速：**1.6×–1.8×**（GEMM-Residual-PartialRMS-GEMM）
- 端到端前向加速：**5%–20%**（Transformer 层）
- LLM 生成 vs 人工性能：**基本持平**

## 相关人物

- **Tri Dao**：FlashAttention 系列核心作者
- **Han Guo**：论文一作

## 相关工作

[[coda-gemm-epilogue-optimization]] — 完整概念解析
[[triattention-kv-compression]] — 同样关注 GPU 内存优化
[[gated-deltanet-2]] — NVIDIA 的注意力机制改进