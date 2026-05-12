---
source_url: https://lilianweng.github.io/posts/2025-05-01-thinking/
ingested: 2026-05-12
sha256: f8a1f24496a828f6075c4f19e4813f34bab48a47d2630513b3f318223d1e992d
---

# Why We Think | Lil'Log

**Author**: Lilian Weng | **Date**: May 1, 2025 | **Reading Time**: 40 min

> A comprehensive review of test-time compute ("thinking time") and chain-of-thought (CoT) reasoning in language models.

---

## Core Motivation

### Psychology Analogy: Dual Process Theory
From *Thinking, Fast and Slow* (Kahneman, 2013):

| System | Characteristics |
|--------|----------------|
| **System 1** (Fast) | Quick, automatic, intuitive; relies on heuristics; prone to errors |
| **System 2** (Slow) | Deliberate, logical, effortful; enables rational decision-making |

CoT enables models to engage in "System 2" thinking—explicitly slowing down to reflect, analyze, and correct mistakes.

### Key Insight: Computation as Resource
- **Transformer flops/token**: ~2× number of parameters
- **Sparse models (MoE)**: `2 × parameters / sparsity`
- **CoT advantage**: Enables variable computation depending on problem hardness

### Latent Variable Perspective
Chain-of-thought can be modeled as latent variable $z$:

$$P(y \mid x) = \sum_{z \sim p(z \mid x)} P(y \mid x, z)$$

This framework explains why collecting multiple parallel CoTs or searching over reasoning paths improves results.

---

## Thinking in Tokens

### Evolution of CoT

| Work | Contribution |
|------|-------------|
| Ling et al. 2017 | Introduced intermediate steps for math (AQUA-RAT dataset) |
| Cobbe et al. 2021 | GSM dataset; generator + verifier approach |
| Nye et al. 2021 | "Scratchpads" for intermediate thinking tokens |
| Wei et al. 2022 | Coined "chain-of-thought" prompting |
| Kojima et al. 2022 | Zero-shot CoT with "think step by step" |

**Key Finding**: Larger models benefit more from thinking time (see Wei et al. 2022 chart).

---

## Branching and Editing

### Parallel Sampling

**Best-of-N**: Simplest approach—sample N times, select highest-scoring.

**Beam Search**: Adaptive search spending more computation on promising paths.

**Process Reward Model (PRM)** guides beam search candidate selection:

> "Per-step self-evaluation reduces accumulative errors in multi-step reasoning during beam search decoding." — Xie et al. 2023

**REBASE** (Wu et al. 2025): Trained PRM determines node expansion depth based on softmax-normalized reward scores.

**Self-Consistency** (Wang et al. 2023): Majority vote among multiple CoT rollouts.

### Sequential Revision

Self-correction **does not work intrinsically** in LLMs. Failure modes:
1. Hallucination (modifying correct responses)
2. Behavior collapse (minor/no modifications)
3. Distribution shift generalization failure

External feedback is required:
- Ground truth matching
- Unit test results
- Stronger model feedback
- Human feedback

#### SCoRe: Self-Correction via Reinforcement Learning

**Two-stage training**:
1. **Stage 1**: Maximize accuracy of 2nd attempt + KL penalty on 1st attempt
2. **Stage 2**: Optimize accuracy of both attempts

> "Stage 1 prevents behavior collapse where the model does minor edits on the first response."

---

## RL for Better Reasoning

### DeepSeek-R1 Training Pipeline

```
Cold-start SFT → Reasoning RL → Rejection Sampling + SFT → Final RL
```

| Stage | Description |
|-------|-------------|
| **Cold-start SFT** | Fine-tune base model on thousands of cold-start data (improves readability) |
| **Reasoning RL** | Rule-based rewards: format (`<thinking>` tags) + accuracy |
| **Rejection Sampling + SFT** | 800k samples, 2 epochs; mix of reasoning + non-reasoning tasks |
| **Final RL** | Improves helpfulness, harmlessness, reasoning |

**Remarkable Finding**: Pure RL (no SFT) can learn advanced reasoning including **reflection and backtracking** ("Aha moment"). The model naturally learns to spend more thinking tokens.

**Failed Attempts** (DeepSeek team):
- **PRM**: Hard to define per-step rubrics; vulnerable to reward hacking
- **MCTS**: Large search space for language; training fine-grained value model is challenging

### Open-Source Replications
- Open-R1, SimpleRL-reason, TinyZero (all on Qwen models)
- Confirmed: Pure RL achieves great math performance + emergent "aha moment"

---

## External Tool Use

| Method | Description |
|--------|-------------|
| **PAL** (Gao et al. 2022) | Offload math/code to code interpreter |
| **Chain of Code** (Li et al. 2023) | LLM as fallback when standard interpreter fails |
| **ReAct** (Yao et al. 2023) | Combines Wikipedia search with reasoning traces |
| **o3 & o4-mini** | Integrate web search, code execution, image processing |

---

## Thinking Faithfully

### CoT Faithfulness Failures (Lanham et al. 2023)

Three perturbation types to test faithfulness:

1. **Early Answering**: Model forms conclusions before CoT generation
2. **Uninformative Tokens**: Filler text (e.g., periods) doesn't improve performance
3. **Human-Unreadable Encoding**: Paraphrasing doesn't degrade accuracy

> **Key Finding**: CoT effectiveness varies by task; thinking time matters more for complex reasoning tasks.

### Reasoning Models Are More Faithful

o3 and o4-mini show improved CoT faithfulness — they actually use the thinking tokens to process information rather than generating post-hoc rationalizations.

---

## Budget Forcing

**Technique** (Dropdown, 2024): Force the model to think more or less by:
- Lengthening: Append "wait" to extend thinking
- Shortening: Append end-of-thinking token or "Final Answer:"

**Finding**: This can improve performance even when the model doesn't naturally extend its CoT. Also enables computing optimal test-time compute allocation.

---

## Compute Optimal Test-Time Scaling

### Scaling Laws for Inference

**Model scaling** (pre-training): More parameters = better performance
**Test-time scaling**: More compute at inference = better performance

**Key Question**: How to optimally allocate test-time compute?

### When Does Test-Time Compute Help?

| Situation | Best Strategy |
|-----------|---------------|
| Easy questions | Direct answer (no CoT) |
| Medium difficulty | Standard CoT |
| Hard reasoning | Search + PRM / Self-Consistency |
| Very hard (search needed) | Beam search, majority voting |

**DeepSeek-R1-Zero**: Shows that very long CoT (64K tokens) emerges naturally when the reward signal is right.

### Compute Allocation Strategies

| Strategy | Description |
|----------|-------------|
| **Verify** | Use outcome reward model (ORM) or process reward model (PRM) to score answers |
| **Search** | Beam search over reasoning paths |
| **Adapt** | Early stopping or extension based on confidence signals |
| **Verify-and-Search** | PRM for step-level guidance + search for answer selection |

---

## Key Takeaways

1. **Thinking tokens enable System 2 reasoning** — explicit reflection and correction
2. **Variable compute** — different problems need different amounts of thinking time
3. **Self-correction requires external feedback** — pure self-correction fails
4. **RL can teach thinking** — DeepSeek-R1 showed emergent "Aha moment" from pure RL
5. **Process reward models are powerful but fragile** — PRM is hard to train and vulnerable to reward hacking
6. **MCTS is challenging for language** — large search space makes fine-grained value estimation difficult
7. **Budget forcing** provides a simple way to control thinking length
8. **Test-time compute scales differently than training compute** — need adaptive strategies

---

## Related Concepts

- [[chain-of-thought-prompting]] — CoT prompting techniques
- [[self-correction]] — LLM self-correction limitations
- [[test-time-compute]] — Scaling inference compute
- [[deepseek-r1]] — DeepSeek-R1 reasoning model
- [[process-reward-model]] — PRM for reasoning
- [[mcts-reasoning]] — Monte Carlo Tree Search for LLM reasoning

---

## References

- Wei et al. 2022 - "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"
- Kojima et al. 2022 - "Large Language Models are Zero-Shot Reasoners"
- Wang et al. 2023 - "Self-Consistency Improves Chain of Thought Reasoning in Language Models"
- Lanham et al. 2023 - "Measuring Faithfulness in Chain-of-Thought Reasoning"
- DeepSeek Team - DeepSeek-R1 training pipeline
- Dropdown 2024 - "Budget Forcing" technique
