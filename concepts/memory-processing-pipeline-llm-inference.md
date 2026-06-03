---
title: Memory Processing Pipeline — LLM 推理内存处理四阶段框架
created: 2026-05-16
updated: 2026-05-16
type: concept
tags: [inference, optimization, architecture]
sources: [raw/articles/2026-05-16-memory-processing-pipeline-llm-inference.md]
confidence: high
---

# Memory Processing Pipeline — LLM 推理内存处理四阶段框架

## 概述

MIT/港大等提出将现代 LLM 推理优化统一为**四阶段内存处理管线**：

```
Prepare Memory → Compute Relevancy → Retrieval → Apply to Inference
```

并证明 GPU-FPGA 异构系统比纯 GPU 方案**快 2.2 倍，能耗降低 4.7 倍**。

## 四阶段详解

| 阶段 | 功能 | 算子特征 |
|------|------|----------|
| **Prepare Memory** | 预处理原始内存为紧凑格式 | 计算密集（算术强度 10-100+），规则访问 |
| **Compute Relevancy** | 为每个内存条目打重要性分 | 内存密集（算术强度 1-10），规则/不规则 |
| **Retrieval** | 按分数选择和提取信息 | 内存密集（算术强度 1），不规则访问 |
| **Apply to Inference** | 将检索内容整合进解码 | 计算密集（算术强度 10-100+），规则访问 |

## 内存处理开销

不同方法中内存处理占总延迟的比例：

- **Sparse Attention**：4K tokens 时 1-11%，1M tokens 时 22-81%
- **RAG**：20M 文档时 40-61%
- **MemAgent**：Prepare Memory 阶段高达 97%
- **Memory as Context**：即使短上下文也很耗时

## 各方法管线映射

| 方法 | Prepare Memory | Compute Relevancy | Retrieval | Apply |
|------|--------------|-------------------|-----------|-------|
| [[triattention-kv-compression\|TriAttention]] | Linear + RoPE | 三角函数级数 | Top-k | 细粒度稀疏注意力 |
| DeepSeek Attention | Linear + RoPE | 多头内积 | Top-k | 细粒度稀疏注意力 |
| SnapKV | Pooling | 内积 | Top-k | 块稀疏注意力 |
| RAG (DRAGIN/FLARE) | Tokenization | BM25 | Top-k | 追加到 query |
| [[test-time-compute-scaling\|Titans]] | Forward pass | 线性投影+内积 | Top-k/加权和 | 追加到段落 |

## GPU-FPGA 异构架构

**为什么 FPGA 适合内存处理：**

- 更大 SRAM 容量，更高有效带宽
- 灵活数据控制，最小化调度开销
- 低功耗（25-45W vs GPU 45-106W）
- 不规则访问模式定制微架构

**分工原则：**
- 计算密集 + 规则访问 → GPU
- 内存密集 + 不规则访问 → FPGA

## 实验结果

AMD MI210 GPU + Alveo U55C FPGA：
- **2.2× 加速**（对比纯 GPU）
- **4.7× 能耗降低**
- 结果可泛化到 NVIDIA A100

## 核心意义

1. **统一框架**：首次将 TriAttention、SnapKV、RAG 等看似不同的方法映射到同一管线
2. **硬件协同**：揭示不同阶段需要不同硬件特性的本质
3. **工程价值**：4.7× 能耗降低对数据中心有重大实际意义

## 相关概念

- [[triattention-kv-compression]] — TriAttention 是该管线在选择性保留方向的具体实现
- [[test-time-compute-scaling]] — 长上下文推理是内存处理开销的主要来源
- [[inference-optimization]] — 推理效率优化是 LLM 落地关键

## 参考

- 论文：https://arxiv.org/abs/2603.29002
- HTML：https://arxiv.org/html/2603.29002v2
