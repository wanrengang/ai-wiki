---
title: ByteDance GRN
created: 2026-05-13
updated: 2026-05-13
type: concept
tags: [model, architecture, visual-generation, bytedance, inference]
sources: [raw/articles/2026-05-13-bytedance-grn-visual-generation.md]
confidence: high
---

# ByteDance GRN

## Overview

**GRN（Generative Refinement Networks）** 是字节跳动商业技术团队研发的全新视觉生成模型，突破了扩散模型（Diffusion）和自回归模型（Autoregressive）两大主流范式，提出第三条路线：让 AI 像人类一样边画边改（continuous draw and revise）。

核心创新：GRN 能够对复杂场景自适应生成更多内容，对简单场景生成更少，同时在生成过程中实时纠错。

## Architecture: Three Components

### 1. Hierarchical Binary Quantization (HBQ)

解决自回归模型中离散 token 化损失问题：
- 将 VAE 编码的连续特征映射到 [-1, +1] 区间
- 使用二叉树结构进行多轮二元量化
- 重建误差按指数分布到更细的量化区间
- 量化误差随轮数指数衰减，实现近无损压缩

两种预测模式：
- **GRN_ind**：将 M 个二元标签合并为单个整数（适合少数量化轮数）
- **GRN_bit**：逐位直接二元预测（适合多轮量化、大模型、视频生成）

两者均使用多 token 并行预测以加速生成。

### 2. Global Refinement Network (GRN)

通过迭代细化解决误差累积/传播问题：

**生成过程**：
1. 从随机 token 图开始
2. 每步两类 token：`[F]`（已生成内容）和 `[R]`（随机 token）
3. 每轮迭代：从上一轮输出随机选取比例转为 `[F]` token
4. 最终：0% → 100% `[F]`（完成）

**关键机制**：
- **Token Erasing**：早期选中的 `[F]` token 可被后续迭代覆盖
- **Token Refinement**：被反复选中的 token 得到逐步精细的预测

这创造了持续的"绘制+修订"循环，**消除了自回归模型固有的误差累积和传播问题**。

### 3. Complexity-Aware Sampling

解决扩散模型"一刀切"计算的低效问题：
- 使用**熵（entropy）**衡量图像复杂度
- 低熵（简单样本）→ 少推理步数
- 高熵（复杂样本）→ 多细化步数

130M 模型从 50 步降至平均 24 步，质量损失极小（gFID 3.56 → 3.79）。

## Performance

| 任务 | 模型规模 | 结果 |
|------|---------|------|
| ImageNet 256×256 C2I | GRN-G 2B | **FID 1.81**, IS 299.0 |
| ImageNet 重建 | HBQ | **rFID 0.56**（超越 SD-VAE 0.87, RAE 0.62） |
| Text-to-Video 480p | GRN 2B | 超越 CogVideoX 5B、Wan 2.1 14B |
| vs MaskGIT | GRN-B 130M | FID 3.56 vs MaskGIT 6.18（参数量减半） |

## 与现有路线对比

| 维度 | Diffusion | Autoregressive | GRN |
|------|-----------|----------------|-----|
| 质量 | 高 | 丢失细节 | 高 |
| 复杂度感知 | ❌ | ✅ | ✅ |
| 纠错能力 | 有限 | ❌ | ✅ |
| 自适应计算 | ❌ | 部分 | ✅ |

## 未来意义

1. **dLLM 潜力**：全局细化方法可帮助文本模型"重新思考"并纠正早期生成
2. **多模态统一**：纯离散 token 方法可更好统一图像、视频和文本 token
3. **可扩展性**：GRN 展现出类似语言模型的离散 token 缩放特性，计划推出更大模型

## 资源链接

- 论文：https://arxiv.org/abs/2604.13030
- 代码：https://github.com/MGenAI/GRN
- 项目主页：https://mgenai.github.io/GRN/
- HuggingFace：https://huggingface.co/spaces/hanjian/GRN

## Related Concepts

- [[test-time-compute-scaling]] — GRN 的迭代细化与 Test-Time Compute Scaling 的自适应计算有相似思路
- [[ai-agent]] — GRN 的"边画边改"机制与 Agent 的反思-修订循环本质相同
- [[cvpr-2026-video-understanding]] — 同为视觉生成领域的架构创新
