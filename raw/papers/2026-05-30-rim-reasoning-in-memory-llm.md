---
source_url: https://arxiv.org/html/2605.30343v1
ingested: 2026-05-30
sha256: 480b063caf4c09d06dee65f1894f1b72ef4b2d091c26e8fb905889d78118c63e
---
# Unlocking the Working Memory of Large Language Models for Latent Reasoning

## 论文信息
- **arXiv**: 2605.30343
- **作者**: Lukas Aichberger, Sepp Hochreiter (JKU Linz, NXAI GmbH)
- **认证**: ACL 2026 Main Conference
- **URL**: https://arxiv.org/html/2605.30343v1

## 核心贡献：Reasoning in Memory (RiM)

### 问题
现有 CoT 推理将中间计算耦合到自回归生成，语言是通信优化的而非计算优化的，导致：
- 推理效率低（需逐 token 生成）
- 算力浪费在生成语法正确的中间文本上

### 核心方法
用**记忆块（Memory Blocks）**替代自回归生成的推理步骤：
1. **记忆块是固定的特殊 token 序列**（不是生成），可在单次前向传播中处理
2. 记忆块激活 LLM 的工作记忆容量（类似人脑的工作记忆）
3. **两阶段课程**：
   - **Stage 1**：在每个记忆块后监督预测下一个推理步骤（建立记忆块与任务的相关性）
   - **Stage 2**：去掉推理步骤监督，改为在每个记忆块后直接精炼最终答案

### 技术细节
- **注意力掩码**：记忆块关注问题和之前的记忆块；目标推理步骤关注之前的记忆块和（可选）问题，但不关注其他推理步骤 → 强制将推理压缩到记忆块内
- **单次前向传播**：所有 readout 可在一次前向传播中训练

### 实验结果
- 训练：GPT-2 和 Llama-3.2 规模，在 GSM8K-Aug 上训练
- 评估：GSM8K 和 GSM-Hard
- **结论**：RiM 匹配或超越现有 CoT 和潜在推理基线，同时实现推理并行化

## 关键洞察
1. **固定 token vs 生成 token**：记忆块固定 → 可并行处理
2. **类人认知**：人类不是"边说边想"，而是使用内部工作记忆
3. **将计算从生成解耦**：推理效率大幅提升

## 标签
#reasoning #latent-reasoning #working-memory #test-time-compute #chain-of-thought