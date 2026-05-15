---
source_url: https://arxiv.org/abs/2510.22977
ingested: 2026-05-12
sha256: 075254142e041738df6f0e13a242d6d7ccb350d0c1347c9c4190472fcdddb77a
---

# The Reasoning Trap: How Enhancing LLM Reasoning Amplifies Tool Hallucination

**Source:** arXiv 2510.22977v2  
**Authors:** Chenlong Yin (Penn State), Zeyang Sha (Nanjing Univ. of Science and Technology), Shiwen Cui, Changhua Meng, Zechao Li  
**GitHub:** https://github.com/albert-y1n/Reasoning_Trap

---

## Core Finding

> *"Stronger reasoning often coincides with increased hallucination, yet no prior work has systematically examined whether reasoning enhancement itself causes tool hallucination."*

**Central Research Question:** Does strengthening reasoning increase tool hallucination of LLM Agents?

**Answer:** **Yes** — across RL, distillation, and toggleable reasoning modes, gains in task performance are consistently accompanied by higher tool hallucination rates.

---

## Key Contributions

1. **SimpleToolHalluBench** — A lightweight diagnostic benchmark for measuring tool hallucination under controlled conditions
2. **First experimental evidence** that reasoning-focused RL inherently amplifies tool hallucination across training methods and model families
3. **Demonstration of reliability-capability trade-off** — reducing hallucination unavoidably degrades utility

---

## SimpleToolHalluBench: The Benchmark

### Design Philosophy

Tests a critical abstention behavior: *"Can agents reliably abstain from tool use when no appropriate tools are available?"*

### Two Task Types

| Task | Description | Failure Mode |
|------|-------------|--------------|
| **No-Tool-Available (NTA)** | System prompt provides no tools, but query requires external tool invocation | Fabricates non-existent tools and outputs |
| **Distractor-Tool (DT)** | Irrelevant distractor tool provided (e.g., calculator for weather query) | Misuses distractor or hallucinates correct tool |

### Construction Details

- **296 tools** selected from AgentSafetyBench
- Queries generated with ChatGPT-4o
- Each query **cannot** be answered via internal model knowledge
- Hallucination rate calculated as:

```
R_NTA = H_NTA / N_NTA
R_DT = H_DT / N_DT
```

Where H = hallucinated responses, N = total samples

---

## Experiment 1: Tool-Specific Reasoning RL

**Setup:** ReCall framework (GRPO-style) on Qwen2.5-7B-Instruct, trained on SynTool

**Finding:** As SynTool validation reward improves, hallucination rates **simultaneously increase substantially** on both NTA and DT tasks.

> *"Agents explicitly rewarded for generating tool-use reasoning chains become over-eager to apply this behavior, even in contexts where tools are missing, irrelevant, or should be abstained from."*

---

## Experiment 2: Non-Agentic Reasoning RL

**Setup:** GRPO training on GSM8K (pure math problems — no tool involvement)

**Results:**

| Model | R_NTA | R_DT |
|-------|-------|------|
| Qwen2.5-7B-Instruct | 34.8% | 54.7% |
| Qwen-7B-GRPO-gsm8k | 90.2% | 100.0% |

**Critical Insight:** Tool hallucination increases **even without tool-related training data**. The root cause is reasoning enhancement itself, not overfitting to tool patterns.

> *"Reinforcing reasoning chains inherently biases models toward generating confident but unfounded outputs, which surface as tool hallucination when external tools are involved."*

---

## Experiment 3: Beyond RL — Distillation & Toggleable Modes

**Method-Agnostic Confirmation:**

| Model | Config | R_NTA (%) | R_DT (%) |
|-------|--------|-----------|----------|
| Qwen2.5-7B | Instruct | 34.8 | 54.7 |
| | R1-Distill | 74.3 | 78.7 |
| Llama3.1-8B | Instruct | 62.5 | 99.7 |
| | R1-Distill | 96.3 | 100 |
| Qwen3-8B | Think Off | 4.1 | 36.2 |
| | Think On | 5.4 | 56.8 |
| Qwen3-32B | Think Off | 5.1 | 46.6 |
| | Think On | 8.8 | 50.7 |

**Conclusion:** Enhanced reasoning consistently correlates with higher hallucination regardless of training method.

---

## Experiment 4: Isolating Reasoning as the Cause

### Ablation: Reasoning vs. RL

| Training Mode | R_NTA | R_DT | Reward |
|---------------|-------|------|--------|
| Qwen2.5-7B-Instruct | 34.8 | 54.7 | 0.22 |
| Direct tool-use RL (no reasoning) | 41.4 | 63.6 | 0.28 |
| Think-then-act RL | 90.2 | 100.0 | 0.45 |

**Finding:** Removing the reasoning step yields only moderate increase (34.8→41.4), but enforcing think-then-act produces dramatic jump to 90.2.

### Ruling Out Instruction-Following Degradation

| Benchmark | Base | +ReCall |
|-----------|------|---------|
| IFEval | 62.4 | 59.8 |
| ComplexBench | 60.8 | 59.4 |
| BFCL Multi-Turn | 13.6 | **23.5** |
| R_NTA | 34.8 | **90.2** |
| R_DT | 54.7 | **100.0** |

**Finding:** Tool-calling competence *improves* (+9.9%) while hallucination *surges*. Tool hallucination is a **distinct failure mode** not captured by existing benchmarks.

---

## Mechanistic Analysis

### Representation Collapse

**Hypothesis:** Reasoning RL causes disproportionately larger representational shifts on tool-related queries.

**Method:** Centered Kernel Alignment (CKA) comparing pre/post-RL model representations

**Finding:**

- **In-distribution (math):** Minimal representational change
- **Out-of-distribution (tool):** Massive collapse toward reasoning-optimal solution

This suggests the model "overwrites" tool-related knowledge with reasoning patterns.

---

## Key Takeaways

1. **The Reasoning Trap:** Reasoning enhancement and tool hallucination are *inherently coupled* — you cannot strengthen one without the other
2. **Abstention is the hard problem:** The benchmark reveals agents can't abstain from tool use when they should
3. **Reliability-capability trade-off is fundamental:** Improving capability inevitably degrades reliability on abstention
4. **Tool hallucination is distinct from tool-calling competence:** Existing benchmarks miss this failure mode
5. **Representation collapse mechanism:** Reasoning RL overwrites tool-related representations

---

## Related Work

- Tool hallucinations previously studied in AgentSafetyBench
- Self-correction approaches (Shinn et al. 2023) require external feedback
- Budget forcing (DeepSeek-R1) lengthens CoT but doesn't address hallucination

---

## BibTeX

```
@article{yin2025reasoningtrap,
  title={The Reasoning Trap: How Enhancing LLM Reasoning Amplifies Tool Hallucination},
  author={Yin, Chenlong and Sha, Zeyang and Cui, Shiwen and Meng, Changhua and Li, Zechao},
  journal={arXiv preprint arXiv:2510.22977},
  year={2025},
  note={Accepted to ACL 2026 Main}
}
```
