---
title: Evo-Depth — VLA 轻量隐式深度编码
created: 2026-05-26
updated: 2026-05-26
type: concept
tags: [VLA, spatial-intelligence, embodied-ai, model, architecture, open-source]
sources: [raw/articles/2026-05-26-evo-depth-vla-spatial-intelligence.md]
confidence: high
---

# Evo-Depth — VLA 轻量隐式深度编码

**Evo-Depth** 是上海交大 MINT 团队提出的轻量级 VLA 空间增强方案，约 0.9B 参数，通过紧凑的隐式深度编码为 VLA 模型补充空间感知能力，在不显著增加系统负担的前提下提升真机成功率。^[raw/articles/2026-05-26-evo-depth-vla-spatial-intelligence.md]

## 核心问题

当前主流 VLA 模型主要依赖二维视觉，在精定位、细摆放、遮挡判断等需要空间感知的任务上成功率明显下滑。现有解决方案各有代价：
- **显式 3D 路线**：依赖深度传感器和点云重建，硬件链路长、对标定误差敏感
- **重隐式 3D 路线**：从 RGB 学习几何，省硬件，但依赖较重的基础模型，训练和推理成本偏高

## 架构：三模块轻量方案

Evo-Depth 由三部分组成：

1. **IDEM（Implicit Depth Encoding Module）**：约 0.13B 参数骨干，从多视角图像提取隐式深度特征，强调空间布局与相对几何关系，而不生成高成本的 3D 中间表示。结合多视角深度预训练初始化，引入深度相关的归纳偏置。

2. **SEM（Spatial Enhancement Module）**：将隐式深度作为调制信号增强视觉-语言表征。相比增加独立深度分支，这种融合方式更克制：VLM 继续负责语义理解，深度特征负责空间增强，尽量控制延迟与显存开销。

3. **Progressive Alignment Training**：分阶段训练（深度表征对齐 → 多模态融合 → 动作学习），解决多模块联合训练的优化不稳定问题。动作头采用当前 VLA 中常见的 **flow-matching** 路线。

## 性能数据

| 维度 | 指标 |
|------|------|
| 仿真 | Meta-World 84.4%、VLA-Arena 41.1%、LIBERO 95.4%、LIBERO-Plus 69.6% |
| 真机 | 平均成功率约 **90%** |
| 部署 | 约 **3.2 GB** GPU 显存、约 **12.3 Hz** 推理频率 |

0.9B 参数即可达到：真机 90% 成功率 + 3.2GB 显存 + 12.3Hz 推理频率，兼顾性能-成本-实时性。

## 核心洞察

Evo-Depth 解决的核心问题是：**如何在不显著增加系统负担的情况下，提升 VLA 的空间能力。** 相比纯二维 VLA 补充了空间信息，相比更重的 3D 路线尽量保留了部署效率。

对于正在做机器人操作、空间智能或 VLA 系统的团队，这类性能-成本-实时性之间的折中方案，可能会越来越重要。

代码与权重已全面开源：https://github.com/MINT-SJTU/Evo-Depth

## 相关概念

- [[faster-vla-action-sampling]] — 港大 VLA 即刻动作采样，解决 VLA 推理延迟问题（TTFA 3倍加速），与 Evo-Depth 同属 VLA 实时性优化方向
- [[vggt-spatial-intelligence]] — VGGT 空间智能，魔芯科技 O(1) 恒定显存流式重建，另一条空间智能路径
- [[embodied-ai-safety-survey]] — 具身智能安全综述，VLA 安全研究的五层能力-风险框架
- [[test-time-compute-scaling]] — 包含 flow-matching 相关推理技术