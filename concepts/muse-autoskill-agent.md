---
title: MUSE-Autoskill Agent
created: 2026-05-27
updated: 2026-05-27
type: concept
tags: [agent, skill, self-evolution, bytedance, benchmarks]
sources: [raw/articles/2026-05-27-muse-autoskill-self-evolving-agents.md]
confidence: medium
---

# MUSE-Autoskill Agent

## Overview

MUSE-Autoskill（Memory-Utilizing Skill Evolution）是字节跳动提出的自进化 Agent 框架，发表在 arXiv 2605.27366。核心思想是将技能（Skill）作为 Agent 的核心资产进行全生命周期管理——创建、记忆、管理、评估、迭代。

## Key Innovation

**将 Skill 从静态工件变为动态生命体**：
- 传统方法：Skill 是孤立的、静态的，一旦创建就不再改变
- MUSE-Autoskill：Skill 有自己的记忆、经验和测试，跨任务持续积累和改进

## Framework: Skill Full Lifecycle

1. **Creation（创建）**：按需从任务中生成 Skill
2. **Memory（记忆）**：每个 Skill 积累跨任务的经验
3. **Management（管理）**：高效组织和选择 Skill
4. **Evaluation（评估）**：通过单元测试和运行时反馈持续优化
5. **Refinement（迭代）**：基于反馈不断提升 Skill 质量

## Skill-Level Memory

区别于 Agent 级别的全局记忆，MUSE-Autoskill 为每个 Skill 维护独立的记忆积累，使 Skill 能在多次任务中持续改进和适应。

## Experiments: SkillsBench

论文在 SkillsBench 上验证了方法有效性，证明：
- 任务成功率提升
- 效率提升
- Skill 重用率提升
- 跨 Agent 迁移能力提升

## Relationship to Other Concepts

- 与 [[moss-source-level-self-evolution]] 的区别：MOSS 强调代码层进化，MUSE 强调 Skill 层的生命周期管理
- 与 [[mem0]] 的关系：两者都关注 Agent 记忆，但 Mem0 是通用记忆层，MUSE 专注 Skill 记忆
- 与 [[agent-memory-processing-pipeline-llm-inference]] 相关：都涉及 LLM 推理中的记忆机制

## Metadata

- Authors: Huawei Lin, Peng Li, Jie Song, Fuxin Jiang, Tieying Zhang
- Affiliation: ByteDance, Rochester Institute of Technology
- arXiv: 2605.27366
- Date: 2026-05-26
- Topics: cs.AI, cs.CL, cs.LG, cs.MA