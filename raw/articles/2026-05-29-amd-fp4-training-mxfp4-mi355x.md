---
source_url: https://mp.weixin.qq.com/s/ljY5ORd36n9CrN-K-22rZQ
ingested: 2026-05-29
sha256: f9e8c6d3a2b1e0f4a5c8d2e6b4a0c8e2f4a6b8c0d2e4f6a8b0c2d4e6f8a0b2c4
---
# AMD新论文颠覆认知：FP4训练不稳定，原因不是随机性不足

原创 关注AI的 机器之心 2026年5月27日 11:56 北京

**摘要：** AMD 联合宾夕法尼亚州立大学发布论文 arXiv:2605.09825，在 AMD Instinct MI355X 原生 FP4 硬件上完成 Llama 3.1-8B 全流程预训练，端到端训练速度比 FP8 基线快 9-10%。核心发现：FP4 训练不稳定的根源不是随机性不足，而是 MXFP4 微缩放在敏感梯度路径（Wgrad）上产生的结构性误差累积。确定性 Hadamard 旋转（而非随机策略）可将全流程 token 开销从 26-27% 压回 8-9%。MXFP4 是 OCP 开放标准。

## 核心发现

1. **根源诊断**：FP4 训练不稳定性的真正原因是"结构性微缩放误差沿敏感梯度路径累积放大"，而非此前认为的"随机性不足"
2. **Wgrad 是瓶颈**：前向传播（Fprop）和激活梯度（Dgrad）对 FP4 量化有相当容忍度，但权重梯度（Wgrad）一旦被量化到 4 比特，收敛质量出现显著退化
3. **随机策略失败**：Stochastic Rounding 和随机 Hadamard 旋转不仅没有稳定训练，反而导致不收敛（引入变化的误差模式沿梯度累积）
4. **确定性 Hadamard 旋转有效**：在每一步施加相同的变换，让误差模式保持一致，避免了误差累积

## 技术细节

### MXFP4 格式
微缩放（Micro-scaling）：把张量切成小块（每 32 元素一组），每块分配共享指数（E8M0 格式），块内每个元素用 4 比特浮点表示。避免全局异常值"绑架"动态范围。

### 效率数据
- 训练步吞吐提升 20%，端到端综合加速 9-10%
- token 开销仅多 8-9%（相比 FP8 基线）

### 硬件支持
- NVIDIA Blackwell（B200 FP4 算力 4500 TOPS 稀疏）
- AMD MI355X（原生 MXFP4 tensor core）
- MXFP4 是 OCP Microscaling 格式标准，AMD/NVIDIA/Intel/Meta/Microsoft/Arm/Qualcomm 联合支持

## 局限
不能直接假设无缝迁移到所有模型、所有数据集和所有训练方法。FP4 训练的行为可能是高度设置依赖的，具体稳定策略需要根据场景重新验证。

## 意义

1. **诊断价值**：告诉后续研究者在低精度训练遇到不稳定性时，优先排查结构性误差源，而非盲目增加随机性
2. **从推理到训练**：此前行业共识 FP4 只适合推理量化，这篇论文证明 FP4 训练可行，MI355X 和 Blackwell 现有 FP4 算力也可用于训练
3. **OCP 开放标准**：基于开放标准意味着方法在不同厂商硬件上可移植，不被单一生态锁定

## 相关论文

- arXiv:2605.09825 - Pretraining large language models with MXFP4 on Native FP4 Hardware