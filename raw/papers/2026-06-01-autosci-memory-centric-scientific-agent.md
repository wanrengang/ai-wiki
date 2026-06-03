---
source_url: https://arxiv.org/abs/2605.31468
ingested: 2026-06-01
sha256: 1ae7296cacb18c4d06a5c01a7d1b0c438ba6d31b77726aed2d1f0f13bb76b675
---

AutoSci: A Memory-Centric Agentic System for the Full Scientific Research Lifecycle

arXiv:2605.31468v1 | Peking University | 2026

## Abstract

AutoSci is a memory-centric agentic system for the full scientific research lifecycle. It is organized around four modules:

- **SciMem**: Schema-governed research memory, separating Long-Term Knowledge Memory for reusable scientific knowledge from Active Research Memory for project-level artifacts (ideas, experiments, manuscripts, reviews)
- **SciFlow**: Harness-based execution framework for the full research lifecycle (literature understanding → ideation → experimentation → writing → rebuttal), controlling state, context, verification, feedback, and orchestration
- **SciDAG**: DAG-shaped multi-agent operators and reusable stage-specific templates for difficult skills
- **SciEvolve**: Converts feedback signals (users, experiments, reviews, external environments) into versioned updates to SciMem organization, SciFlow skills, and SciDAG templates

AutoSci is the first system to provide all four capabilities: full-lifecycle support + execution harness + structured persistent memory + system evolution.

## Key Features (vs. prior art)

Table 1 comparison shows AutoSci is the ONLY system that has all of:
- Harness (persistent state, controlled context, verification gates, recoverable orchestration)
- Structured persistent scientific memory (organized as typed objects with explicit dependencies, not just text summaries)
- Self-evolution (modifying skills/workflows themselves, not just accumulating textual experience)

Prior systems like AI Scientist series, Agent Laboratory, ARIS, NORA, Deep Researcher Agent all lack at least one of these.

## Architecture

- **SciMem**: Two-tier — Long-Term Knowledge Memory (reusable cross-project) + Active Research Memory (project-local artifacts)
- **SciFlow**: Five-stage pipeline with harness controlling state/context/verification/feedback/orchestration
- **SciDAG**: DAG-based multi-agent for stages requiring search, debate, verification, or refinement
- **SciEvolve**: Versioned updates driven by feedback signals

## Case Studies

1. **GPU kernel optimization**: AutoSci generates reviewable paper-level artifacts
2. **Biomedical drug discovery**: Full idea→experiment→manuscript pipeline

Both produce artifacts that receive automated ICLR-style reviews.

## Related Systems

- AI Scientist (Lu et al., 2024; Yamada et al., 2025) — no persistent memory, no self-evolution
- Agent Laboratory (Schmidgall et al., 2025) — harness but no structured persistent memory
- ARIS, NORA, Deep Researcher Agent — harness + memory but no system evolution
- EvoScientist (Lyu et al., 2026) — partial evolution (textual memory only)

GitHub: https://github.com/skyllwt/AutoSci
