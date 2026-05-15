---
title: TriAttention — KV Cache 选择性压缩
created: 2026-05-14
updated: 2026-05-15
type: concept
tags: [model, architecture, inference, optimization]
sources: [raw/articles/2026-05-14-triattention-kv-compression-nvidia-mit-zhejiang.md, raw/articles/2026-05-15-triattention-nvidia-mit-zju.md]
confidence: high
---

# TriAttention — KV Cache 选择性压缩

## 概述

TriAttention 是 MIT、英伟达、浙大联合提出的 KV cache 选择性压缩方法，核心创新是在 pre-RoPE 空间通过 Q/K 三角集中度估计 token 重要性，只保留关键 KV pair，实现 **10.7 倍内存缩减**、**2.5 倍吞吐量提升**，同时匹配 Full Attention 准确率。

## 核心思想

现有 KV cache 压缩两条路线：
- **量化派**（Google TurboQuant）：把每个行李都压扁，降低 bit 宽度
- **选择性保留派**（TriAttention）：先判断哪些 token 值得留、哪些可以扔，减少行李数量

两种路线理论上可以叠加。TriAttention 的思路类比：别的方法像把所有行李都塞进压缩袋，TriAttention 是先翻一遍行李箱，把砖头扔掉，只给羽绒服打包。

## 核心结果

| 指标 | 数值 |
|------|------|
| AIME25 准确率 | 40.8%（与 Full Attention 持平） |
| 吞吐量提升 | 2.5 倍 |
| KV cache 内存缩减 | 10.7 倍 |

## 消费级硬件部署

单张 RTX 4090（24GB）成功运行 Qwen3-32B（AWQ INT4 量化）+ OpenClaw agent 工作流，读取 6 份 markdown 文档生成完整周报——首次证明完整的、有实际生产价值的 agent 任务可以在消费级硬件上跑通。

## 工程落地

- **vLLM 插件**：挂上就能用，不需要改模型架构或重新训练
- **Apple Silicon**：MLX 实验性支持（M1-M4），但尚未成熟

## 技术细节

- **pre-RoPE 空间分析**：在 RoPE 位置编码应用之前分析 Q/K 分布
- **三角集中度度量**：用三角函数性质估计 token 对最终输出贡献度
- **重要性估计**：区分"羽绒服"（重要）和"砖头"（可丢弃）token

## 相关概念

- [[test-time-compute-scaling]] — 长序列推理与 Test-Time Scaling 方向相关，KV 压缩是其关键技术支撑
- [[openclaw]] — 论文实验基于 OpenClaw agent 工作流
- [[vibe-trading]] — vLLM 插件方向，量化推理引擎

## 作者

- Weian Mao（MIT CSAIL 博士后，博士毕业于阿德莱德大学 AIML，师从沈春华）
- Xi Lin（浙江大学计算机科学与技术专业高年级本科生）
- Wei Huang（香港大学博士生，NVIDIA Research 实习，Song Han 指导）

## 参考

- 论文：https://arxiv.org/abs/2604.04921
- GitHub：https://github.com/WeianMao/triattention
