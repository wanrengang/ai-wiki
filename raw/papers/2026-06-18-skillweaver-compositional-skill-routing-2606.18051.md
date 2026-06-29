---
source_url: https://arxiv.org/html/2606.18051v1
ingested: 2026-06-18
sha256: 80ae88e5199d7a9bc4de7c6b73c18118ed5587a5670b6837330f6b8e848baaa7
---

Compositional Skill Routing for LLM Agents: Decompose, Retrieve, and Compose
Xueping Gao
Alibaba Cloud Hangzhou, China hellogxp@gmail.com

Abstract

LLM agents increasingly rely on external skills—reusable tool specifications—but real-world tasks often require composing multiple skills, not just selecting one. We formalize this as the Compositional Skill Routing problem: given a complex user query and a large skill library, decompose the query into atomic sub-tasks, retrieve the appropriate skill for each sub-task, and compose an executable plan. We present SkillWeaver, a decompose-retrieve-compose framework combining an LLM task decomposer, a bi-encoder skill retriever with FAISS indexing, and a dependency-aware DAG planner. To support evaluation, we introduce CompSkillBench, a benchmark of 300 compositional queries over 2,209 real MCP server skills spanning 24 functional categories, sourced from the public MCP ecosystem. Our experiments reveal that task decomposition quality is the primary bottleneck: standard LLM decomposition reaches only 34.2% category recall at the step level. To address this, we propose Iterative Skill-Aware Decomposition (SAD), a retrieval-augmented feedback loop that iteratively aligns decomposition with available skills. SAD improves decomposition accuracy from 51.0% to 67.7% (+32.7%, Wilcoxon p<10−6) in a single iteration; DA-conditioned analysis confirms that correct granularity is the prerequisite for effective retrieval (CatR@1 rises from 34% to 41% when DA=1). SkillWeaver reduces context window consumption by over 99%, and transfer experiments confirm generalization (+35.6% relative DA gain even when target categories are absent from the retrieval pool).

1 Introduction

The agent paradigm for large language models (LLMs) has evolved beyond single-turn generation to encompass tool use, planning, and multi-step task execution Schick et al. (2023); Qin et al. (2023); Patil et al. (2024). A key architectural pattern emerging in modern LLM agents is the use of skills: modular, reusable tool specifications that define specific capabilities along with instructions for when and how to invoke them Anthropic (2025). We use skill following Anthropic's SKILL.md specification; skills differ from traditional APIs in their emphasis on structured natural language documentation and composability metadata. As agent skill libraries grow—with repositories already containing thousands of community-contributed skills—a fundamental routing question arises: given a user query, which skill(s) should the agent invoke?

Prior work treats skill routing as single-skill selection Zheng et al. (2025), but real-world queries frequently require multiple skills—e.g., "Download the dataset, transform it, and create visual reports" needs an API client, a data processor, and a visualization tool.

We formalize this as Compositional Skill Routing: given query q and skill library S, produce an ordered sequence of skills [s1,...,sk] where each yi handles one atomic sub-task.

We present SkillWeaver, a three-stage framework: (1) Decompose: An LLM-based task decomposer that breaks complex queries into atomic sub-tasks. (2) Retrieve: A bi-encoder retriever that identifies candidate skills for each sub-task using semantic similarity. (3) Compose: A compatibility-aware planner that selects skills per step using inter-skill compatibility.

We introduce CompSkillBench: 300 compositional queries over 2,209 real MCP server skills spanning 24 functional categories.

Key findings: (1) Decomposition is the bottleneck: Standard LLM decomposition achieves only 34.2% CatR@1 on 2,209 real skills. (2) SAD closes the gap: Iterative Skill-Aware Decomposition improves DA from 51.0% to 67.7% (+32.7%, p<10−6) in a single iteration. (3) Metadata suffices for retrieval: Metadata-only encoding achieves CatR@10 of 69.0%. (4) SAD generalizes: Transfer experiments show +35.6% relative DA gain even when target categories are absent from the retrieval pool.

7 Results

Main results on CompSkillBench (2,209 skills, 24 categories, 300 queries):
- Vanilla (Qwen2.5-7B): DA=51.0%, CatR@1=34.2%, CatR@10=68.6%
- +SAD (H=15): DA=67.7%, CatR@1=37.0%, CatR@10=70.3%

Difficulty breakdown: Easy DA 44.7%→63.3% (+41.6%), Medium 66.0%→78.0% (+18.2%), Hard 40.0%→60.0% (+50.0%).

LLM-Direct ceiling (qwen-max, 100 skills shown): DA=90% but CatR@1=21.1% — even a much stronger model cannot match retrieval-based routing with SAD, confirming the skill matching challenge is not merely a model capacity problem.

ReAct-style: DA=0% because the think-act-observe loop collapses multi-step tasks into single actions without explicit decomposition guidance.
