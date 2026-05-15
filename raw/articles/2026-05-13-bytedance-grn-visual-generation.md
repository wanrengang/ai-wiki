---
source_url: https://lilianweng.github.io/posts/2025-05-01-thinking/
ingested: 2026-05-13
sha256: 81772ea3c53ff0cb427e82f8e2d5ed45406f8aac92ae5bf0ca6f7f0af541004c
---

# Why We Think | Lilian Weng (2025-05-01)

## Overview

Lilian Weng 2025年5月发布的深度文章，系统解析大语言模型的推理（Thinking）与思考（Reasoning）机制。

## Core Topics

### Self-Correction in LLM Reasoning

Naively applying self-correction leads to worse performance — external feedback is needed for models to self-improve. Sources of feedback:
- Matching ground truths
- Heuristics and task-specific metrics
- Unit tests results for coding questions (Shinn et al. 2023)
- A stronger model as corrector (Zhang et al. 2023)

### Scaling CoT Reasoning Path Length

Budget Forcing technique (Zhang et al. 2025):
- **Lengthen**: forcefully append the word "wait" to extend reasoning
- **Shorten**: terminate model's thinking by appending end-of-thinking token or "Final Answer:"

### Monitoring Reasoning Models

Monitoring CoT of reasoning models can effectively detect model misbehavior such as reward hacking, and can even enable a weaker model to monitor a stronger model.

### Adaptive Computation

Combines self-attention in Transformer with recurrent mechanism in RNN, dynamically adjusting the number of steps using Adaptive Computation Time (Graves, 2016).

---

## Key References

- Zhang et al. (2025) - Budget Forcing on CoT scaling
- Shinn et al. (2023) - Reflexion: Language Agents with Verbal Reinforcement Learning
- Zhang et al. (2023) - Learning to (Learn at Test-Time)
- Graves (2016) - Adaptive Computation Time

---

## URL

https://lilianweng.github.io/posts/2025-05-01-thinking/
