---
source_url: https://arxiv.org/abs/2606.19348
ingested: 2026-06-19
sha256: afd0b411cab0c2cfc285ed3182d8c178b9df169217461b6cacf4e366bb385f2a
---

DeepSeek-V4: Towards Highly Efficient Million-Token Context Intelligence
DeepSeek-AI
research@deepseek.com

Abstract

We present a preview version of DeepSeek-V4 series, including two strong Mixture-of-Experts (MoE) language models — DeepSeek-V4-Pro with 1.6T parameters (49B activated) and DeepSeek-V4-Flash with 284B parameters (13B activated) — both supporting a context length of one million tokens. DeepSeek-V4 series incorporate several key upgrades in architecture and optimization: (1) a hybrid attention architecture that combines Compressed Sparse Attention (CSA) and Heavily Compressed Attention (HCA) to improve long-context efficiency; (2) Manifold-Constrained Hyper-Connections (mHC) that enhance conventional residual connections; (3) and the Muon optimizer for faster convergence and greater training stability. We pre-train both models on more than 32T diverse and high-quality tokens, followed by a comprehensive post-training pipeline that unlocks and further enhances their capabilities. DeepSeek-V4-Pro-Max, the maximum reasoning effort mode of DeepSeek-V4-Pro, redefines the state-of-the-art for open models, outperforming its predecessors in core tasks. Meanwhile, DeepSeek-V4 series are highly efficient in long-context scenarios. In the one-million-token context setting, DeepSeek-V4-Pro requires only 27% of single-token inference FLOPs and 10% of KV cache compared with DeepSeek-V3.2. This enables us to routinely support one-million-token contexts, thereby making long-horizon tasks and further test-time scaling more feasible. The model checkpoints are available at https://huggingface.co/collections/deepseek-ai/deepseek-v4.

Key Technical Details

Architecture:
- Retains DeepSeekMoE framework and Multi-Token Prediction (MTP) strategy from V3
- Hybrid attention: Compressed Sparse Attention (CSA) + Heavily Compressed Attention (HCA)
- Manifold-Constrained Hyper-Connections (mHC) strengthen residual connections
- Muon optimizer for faster convergence and training stability
- Activation function change: Sigmoid → Sqrt(Softplus())
- Hash routing for initial Transformer blocks (replaces dense FFN)

Efficiency Breakthrough:
- DeepSeek-V4-Pro: 27% single-token inference FLOPs vs V3.2, 10% KV cache
- DeepSeek-V4-Flash: 10% FLOPs, 7% KV cache vs V3.2 (1M token context)
- FP4 quantized routed expert weights

Training:
- DeepSeek-V4-Flash: 32T tokens pre-training
- DeepSeek-V4-Pro: 33T tokens pre-training
- Post-training: two-stage (domain-specific expert SFT+RL → unified model on-policy distillation)

Evaluation Results:
- DeepSeek-V4-Pro-Max: SOTA among open models, surpasses Gemini-3.1-Pro on 1M-token academic benchmarks
- Reasoning: outperforms GPT-5.2 and Gemini-3.0-Pro, trails GPT-5.4/Gemini-3.1-Pro by ~3-6 months
- Agent: on par with Kimi-K2.6 and GLM-5.1; outperforms Claude Sonnet 4.5, approaches Opus 4.5
- Knowledge: significantly outperforms leading open-source models on SimpleQA; closes gap with Gemini-3.1-Pro on MMLU-Pro/HLE/GPQA

Models:
- DeepSeek-V4-Pro: 1.6T total / 49B activated parameters
- DeepSeek-V4-Flash: 284B total / 13B activated parameters
- Both support 1M token context natively