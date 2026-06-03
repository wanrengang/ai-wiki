---
source_url: https://arxiv.org/html/2603.29002v2
ingested: 2026-05-16
sha256: ef9c2c3d4a5b6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f
---

# Understanding and Accelerating Memory Processing Pipeline for Large Language Model Inference

## Overview

This paper demonstrates that modern LLM inference optimizations (sparse attention, RAG, compressed contextual memory) can be unified into a **four-stage memory processing pipeline** and proposes a **GPU-FPGA heterogeneous system** to accelerate it.

## Key Claims

| Claim | Finding |
|-------|---------|
| **Claim 1** | Memory processing accounts for **22%–97%** of total LLM inference latency |
| **Claim 2** | Memory processing operations are **heterogeneous** (compute-bound vs memory-bound, regular vs irregular access) |
| **Claim 3** | Heterogeneous systems (GPU+FPGA) can effectively accelerate memory processing |

## Four-Stage Memory Processing Pipeline

```
┌─────────────────┐     ┌──────────────────┐     ┌───────────┐     ┌───────────────────┐
│  Prepare Memory │ ──▶ │ Compute Relevancy│ ──▶ │ Retrieval │ ──▶ │ Apply to Inference│
│    prep(M<t)    │     │    comp(I<t,xt)  │     │ ret(...)  │     │   apply(M't,xt)   │
└─────────────────┘     └──────────────────┘     └───────────┘     └───────────────────┘
```

| Stage | Description | Example |
|-------|-------------|---------|
| **Prepare Memory** | Preprocess raw memory into compact/structured format | Linear projections, RoPE embedding, tokenization |
| **Compute Relevancy** | Assign importance scores to each memory entry | Multi-headed inner product, BM25 scoring |
| **Retrieval** | Select and extract information based on scores | Top-k selection, threshold-based selection |
| **Apply to Inference** | Integrate retrieved content into decoding | Sparse attention, query augmentation |

## Pipeline Mapping for Different Methods

| Method | Prepare Memory | Compute Relevancy | Retrieval | Apply to Inference |
|--------|---------------|-------------------|-----------|-------------------|
| **TriAttention** | Linear Projections + RoPE | Trigonometric Series | Top-k | Fine-grain Sparse Attention |
| **DeepSeek Attention** | Linear Projections + RoPE | Multi-headed Inner Product | Top-k | Fine-grain Sparse Attention |
| **SeerAttention-R** | Linear Projections + Pooling | Inner Product | Top-k / Threshold | Block Sparse Attention |
| **LServe** | Page-wise Min/Max Pooling | Inner Product + Max Reduction | Top-k | Block Sparse Attention |
| **RAG (DRAGIN, FLARE)** | Tokenization | BM25 | Top-k | Append to query |
| **MemAgent** | Model Decoding | N/A | Nearest Retrieval | Model Prefilling |
| **Titans (Memory as Context)** | Forward pass | Linear Projection + Inner Product | Top-k / Weighted Sum | Append to segment |
| **TTT/LaCT** | Backward pass | Compute loss | N/A | Forward pass |

## Memory Processing Overhead Analysis

**Profiling reveals substantial latency breakdown:**

- **Sparse Attention**: Memory processing grows from **1–11%** (4K tokens) to **22–81%** (1M tokens)
- **RAG**: **40–61%** of latency at 20M documents
- **MemAgent**: Up to **97%** of latency in Prepare Memory stage
- **Memory as Context**: Time-consuming even with short contexts due to multiple linear layers

## Computational Heterogeneity

| Property | Prepare Memory | Compute Relevancy | Retrieval | Apply to Inference |
|----------|---------------|-------------------|-----------|-------------------|
| **Arithmetic Intensity** | 10–100+ (compute-bound) | 1–10 (memory-bound) | 1 (memory-bound) | 10–100+ (compute-bound) |
| **Access Pattern** | Regular | Regular/Irregular | Irregular | Regular |
| **Data Requirement** | Local Memory | Across Memories | Across Memories | Local Memory |

**Key insight**: Different stages require different hardware optimizations:
- **Compute-bound, regular**: Best on GPU
- **Memory-bound, irregular, data-dependent**: Better on FPGA

## GPU-FPGA Heterogeneous System

### Why FPGA for Memory Processing?

1. **Larger SRAM capacity** with higher effective bandwidth
2. **Flexible data control** with minimized scheduling overhead
3. **Low power consumption** (~25-45W vs GPU's 45-106W)
4. **Custom microarchitecture** for irregular data access patterns

### System Mapping Strategy

**General Setup (Sparse Attention, RAG):**
```
GPU                    FPGA
─────────────────────────────────────
Prepare Memory     ──▶ Compute Relevancy
                    +  Retrieval
Apply to Inference ◀── Return indices
```

**MemAgent:**
```
GPU                    FPGA
─────────────────────────────────────
Apply Memory (Prefill) ◀── KV cache
                    ──▶ Prepare Memory (Decode)
                    ◀── Token IDs
```

**Memory as Context:**
- FPGA stores memory, handles query generation + cross attention
- GPU stores model weights and handles main inference

## Experimental Results

Evaluated on AMD MI210 GPU + Alveo U55C FPGA:
- **2.2× speedup** vs GPU baseline
- **4.7× energy reduction** across multiple LLM inference optimizations
- Results generalize to NVIDIA A100

## Key Takeaways

1. KV compression methods (TriAttention, SnapKV, DeepSeek) all map to same 4-stage pipeline
2. Different stages have fundamentally different hardware requirements
3. FPGA is well-suited for memory-bound, irregular retrieval stages
4. GPU-FPGA heterogeneous design outperforms homogeneous GPU-only approach
5. Energy efficiency gain of 4.7× makes this direction practically important for data centers

## Reference

- Paper: https://arxiv.org/abs/2603.29002
- arXiv HTML: https://arxiv.org/html/2603.29002v2
