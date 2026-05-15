---
source_url: https://arxiv.org/html/2601.02997v1
ingested: 2026-05-13
sha256: 18e0a163628f74b7b6fffd75f375f3bc422eea19e1bcfd907270d78ccb7e0142
---

# From Memorization to Creativity: LLM as a Designer of Novel Neural-Architectures

**Source:** arXiv:2601.02997v1
**Authors:** Waleed Khalid, Dmitry Ignatov, Radu Timofte (University of Würzburg, Germany)
**Submitted:** January 2026

---

## Research Question

Can an LLM be iteratively fine-tuned to become a robust generator of valid, high-performing, and structurally novel neural network architectures?

## Key Finding

> *"LLMs can internalize empirical, non-textual rewards to transcend their training data"*

The LLM successfully evolved from a stochastic code generator into a robust architectural prior capable of producing diverse, high-quality PyTorch convolutional networks for CIFAR-10 classification.

## Framework Overview

A closed-loop synthesis system with 22 cycles of:
1. **Generate** → LLM samples PyTorch architectures
2. **Validate** → Syntax/compilation checks
3. **Evaluate** → Single-epoch CIFAR-10 accuracy
4. **Filter** → MinHash-Jaccard novelty detection
5. **Fine-tune** → LoRA adaptation on successful generations

## Pipeline Architecture

### Prompting Strategy
- **Model:** DeepSeek-Coder-7B-Instruct-v1.5 (LoRA-fine-tuned)
- **API Contract:** Fixed `Net(nn.Module)` class with `__init__`, `forward`, `train_setup`, `learn`, `supported_hyperparameters()`
- **Constraints:**
  - Input: `(N,3,32,32) → 10` logits
  - Max 500K parameters
  - Standard operations only (conv/pool/norm/activations)
  - No pretrained weights

### Evaluation Protocol
- **Single-epoch CIFAR-10 accuracy** as low-fidelity proxy
- Selection threshold: **≥40% first-epoch accuracy**

### Novelty Filtering
- **Token-level shingling** (k=10)
- **MinHash signatures** (256 permutations)
- **LSH retrieval** (threshold 0.85)
- **Near-duplicate threshold:** τ = 0.90

## Results Summary

| Metric | Cycle 1 | Cycle 10 | Cycle 18 | Cycle 22 |
|--------|---------|----------|----------|----------|
| Valid generation rate | 44.0% | 53.8% | 59.1% | 41.8% |
| Best accuracy | 47.78% | 55.48% | **63.98%** | 57.62% |
| Mean accuracy | 28.06% | 37.70% | **50.99%** | 49.48% |
| ≥40% accuracy | 2.04% | 38.04% | **96.81%** | 92.86% |
| Novel models added | 1 | 18 | 38 | 30 |

### Aggregate Statistics
- **Overall mean accuracy:** 42.32% [41.80%, 42.83%]
- **Mean valid rate:** 50.6% [45.0%, 56.1%]
- **Peak valid rate:** 74.5% (Cycle 14)
- **Total unique architectures added:** 455

## Technical Specifications

### LoRA Configuration
- rank r = 32, α = 32, dropout = 0.05
- Target modules: q/k/v/o + MLP (up_proj, down_proj, gate_proj)
- Layers: 24 Transformer layers (0-23)

### Fine-tuning Hyperparameters
- Learning rate: 1×10⁻⁵, Epochs per cycle: 5
- Optimizer: 8-bit paged AdamW, Precision: bfloat16

## Significance

> *"The generator's behavior can be shaped in a systematic and measurable manner under controlled conditions"*

LLMs can transition from **memorization** to **creativity** when grounded in execution feedback.

## Limitations
1. Single dataset/task (CIFAR-10 only)
2. Proxy metric (first-epoch accuracy may favor fast learners)
3. Fixed threshold selection policy
4. Text-level novelty doesn't capture semantic equivalence at computation graph level
