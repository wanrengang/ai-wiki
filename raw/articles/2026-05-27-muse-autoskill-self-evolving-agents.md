---
source_url: https://arxiv.org/abs/2605.27366
ingested: 2026-05-27
sha256: a7c8e9d0f1b2a3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8
---

# MUSE-Autoskill: Self-Evolving Agents via Skill Creation, Memory, Management, and Evaluation

- Authors: Huawei Lin, Peng Li, Jie Song, Fuxin Jiang, Tieying Zhang (ByteDance, Rochester Institute of Technology)
- arXiv: 2605.27366 [cs.AI]
- Date: 2026-05-26
- Comments: 30 pages, 8 figures, 13 tables
- Subjects: cs.AI, cs.CL, cs.LG, cs.MA

## Abstract

Large language model (LLM) agents rely on reusable skills to solve complex tasks. However, existing skill creation approaches treat skills as isolated and static artifacts, limiting their reusability, reliability, and long-term improvement. We propose MUSE-Autoskill Agent (Memory-Utilizing Skill Evolution), a skill-centric agent framework that lets agents continuously improve their task-solving capability by creating, reusing, and refining skills under a unified lifecycle (creation, memory, management, evaluation, and refinement). Our framework enables agents to create skills on demand, store and reuse them across tasks, organize and select them efficiently, and evaluate them through unit tests and runtime feedback for continuous refinement. We further introduce skill-level memory that accumulates experience for each skill across tasks, enabling more effective reuse and adaptation over time. Experiments on SkillsBench provide initial evidence that lifecycle-managed skills can improve task success, efficiency, reuse, and cross-agent transfer, highlighting the importance of treating skills as long-lived, experience-aware, and testable assets.

## Key Contributions

1. **MUSE-Autoskill Agent Framework**: Skill-centric agent framework with unified lifecycle management
2. **Skill-level Memory**: Accumulates experience for each skill across tasks, enabling effective reuse
3. **SkillsBench Benchmark**: New benchmark for evaluating skill creation and management
4. **Empirical Validation**: Lifecycle-managed skills improve task success, efficiency, reuse, and cross-agent transfer

## Framework Overview

MUSE-Autoskill introduces a full skill lifecycle:
- **Creation**: On-demand skill generation based on task requirements
- **Memory**: Skill-level memory accumulation across multiple tasks
- **Management**: Efficient organization and selection of skills
- **Evaluation**: Unit tests and runtime feedback for continuous refinement

The framework treats skills as "long-lived, experience-aware, and testable assets" — a departure from static skill definitions in existing agent systems.

## Experimental Results

Experiments on SkillsBench show improvements across:
- Task success rate
- Efficiency
- Skill reuse rate
- Cross-agent transfer capability

## References

- arXiv: https://arxiv.org/abs/2605.27366
- PDF: https://arxiv.org/pdf/2605.27366