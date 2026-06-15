---
source_url: https://arxiv.org/abs/2606.06473
ingested: 2026-06-08
sha256: 438c4e94164a9ebe2388d38f0e4d262d26328c2daaf7aeca0f33d55996e94d08
---

MLEvolve: A Self-Evolving Framework for Automated Machine Learning Algorithm Discovery

Authors: Shangheng Du, Xiangchao Yan, Jinxin Shi, Zongsheng Cao, Shiyang Feng, Zichen Liang, Boyuan Sun, Tianshuo Peng, Yifan Zhou, Xin Li, Jie Zhou, Liang He, Bo Zhang, Lei Bai

Affiliations: Shanghai Artificial Intelligence Laboratory; East China Normal University

Abstract

Large language model (LLM) agents are increasingly applied to long-horizon tasks such as scientific discovery and machine learning engineering (MLE), where sustained self-evolution becomes a key capability. However, existing MLE agents suffer from inter-branch information isolation, memoryless search, and lack of hierarchical control, which together hinder long-horizon optimization. We present MLEvolve, an LLM-based self-evolving multi-agent framework for end-to-end machine learning algorithm discovery. By extending tree search to Progressive MCGS, MLEvolve enables cross-branch information flow through graph-based reference edges and gradually shifts the search from broad exploration to focused exploitation with an entropy-inspired progressive schedule. To allow the agent to evolve with accumulated experience, we introduce Retrospective Memory, which combines a cold-start domain knowledge base with a dynamic global memory for task-specific experience retrieval and reuse. For stable long-horizon iteration, we further decouple strategic planning from code generation with adaptive coding modes. Evaluation on MLE-Bench shows that MLEvolve achieves state-of-the-art performance across multiple dimensions including average medal rate and valid submission rate under a 12-hour budget (half the standard runtime). Moreover, MLEvolve also outperforms specialized algorithm discovery methods including AlphaEvolve on mathematical algorithm optimization tasks, demonstrating strong cross-domain generalization. Our code is available at https://github.com/InternScience/MLEvolve.

1 Introduction

Artificial intelligence (AI) is reshaping scientific research and complex engineering, leading to the paradigm of AI for Science. With the continued advancement of large language models (LLMs), LLM-based agent systems are now being applied to long-horizon autonomous tasks such as scientific discovery, automated experimentation, and end-to-end algorithm design. Unlike single-turn reasoning, these scenarios involve open search spaces and limited time budgets, where agents must continually generate solutions, execute code, evaluate outcomes, and adjust strategies based on feedback. During this process, the agent continuously evolves: accumulating experience from past trials, adaptively adjusting exploration strategies, and progressively refining implementations according to the current search stage. This sustained self-evolving capability is becoming central to long-horizon autonomous agents.

Machine Learning Engineering (MLE) is one of the most representative scenarios for such long-horizon self-evolving tasks. Designing high-performance AI systems still relies heavily on expert knowledge and extensive manual iteration. Although recent advances in AutoML have achieved significant progress in optimizing discrete stages such as data processing and model selection, they often fall short of covering the entire end-to-end MLE pipeline, from data preparation to model training and inference. Recently, LLM-based coding agents have been applied to MLE scenarios, using the planning and code generation capabilities of LLMs to iteratively optimize within open search spaces.

Despite these advances, existing MLE agents face three key challenges that hinder self-evolution over long horizons:
1. Inter-branch information isolation: Most methods adopt linear or tree-structured search, confining information within individual branches and making it difficult to transfer successful strategies across different search trajectories.
2. Memoryless search: Current search frameworks propagate only scalar rewards, resulting in each planning decision being made in isolation without reusing insights from similar attempts.
3. Lack of hierarchical control: Many methods rewrite the entire solution at every iteration, resulting in low iteration efficiency and uncontrollable modifications.

To bridge this gap, we present MLEvolve, an LLM-based self-evolving multi-agent framework for MLE tasks with three core components:
1. Progressive Monte Carlo Graph Search (MCGS): Addresses isolation through graph-based cross-branch information flow, and introduces an entropy-inspired progressive exploration schedule that adaptively steers the search from broad exploration to focused exploitation over time.
2. Retrospective Memory: Pairs a curated domain knowledge base for cold-start initialization with a dynamic global memory that automatically accumulates and retrieves task-specific experience throughout the search.
3. Hierarchical Planning with Adaptive Code Generation: Separates strategic planning from code generation and selects among full rewrite, stepwise, and diff-based editing modes according to the current search state.

Experimental results show that MLEvolve achieves a 65.3% average medal rate on MLE-Bench under a 12-hour budget (half the standard runtime), establishing state-of-the-art performance, and further outperforms specialized algorithm discovery methods including AlphaEvolve on mathematical optimization tasks.

3 MLEvolve

3.1 Problem Formulation

The objective is to automate the search, design, and optimization of end-to-end ML pipelines. Each node represents a complete candidate solution covering preprocessing, feature engineering, model training, and prediction.

3.2 Progressive MCGS

3.2.1 Graph-based Search Space

The search process is organized as a directed graph G = (V, E), E = E_T∪ E_ref, where:
- Primary edges E_T: (u, v) ∈ E_T means v is derived from u by applying an operator o, i.e., v = g_o(u, R). These edges preserve parent-child generative order and are used for selection and backpropagation.
- Reference edges E_ref: (r, v) ∈ E_ref denotes that v additionally incorporates information from node r beyond its parent node. These edges connect nodes across branches or non-adjacent levels, enabling cross-branch knowledge flow and compositional transfer, but do not participate in backpropagation.

3.2.2 Progressive Exploration Schedule

An entropy-inspired progressive schedule adaptively balances exploration vs. exploitation over time — early stages favor broad exploration, later stages focus on exploitation.

3.3 Retrospective Memory

Combines:
- Cold-start domain knowledge base: curated static knowledge for initialization
- Dynamic global memory: automatically accumulates and retrieves task-specific experience during search, without requiring additional LLM calls for explicit reflection

3.4 Hierarchical Planning with Adaptive Code Generation

Decouples strategic planning from code implementation. Selects among three coding modes:
- Full rewrite: complete solution rewrite
- Stepwise: incremental modification
- Diff-based editing: targeted edits

4 Experiments

MLEvolve achieves 65.3% average medal rate on MLE-Bench under 12-hour budget (half standard runtime) — SOTA. Also outperforms AlphaEvolve on mathematical optimization tasks, demonstrating cross-domain generalization.

Related Work

- AIDE: greedy search for MLE
- ML-Master: MCTS-based
- AIRA-Dojo: MCTS-based
- MARS: budget-aware MCTS with contrastive reflection
- FM-Agent: evolutionary multi-island parallel search
- R&D-Agent: researcher-developer multi-agent combination
- AutoMind: domain knowledge base grounding
- ML-Master 2.0: hierarchical cognitive caching
- AlphaEvolve: specialized algorithm discovery for math

arXiv: https://arxiv.org/abs/2606.06473
GitHub: https://github.com/InternScience/MLEvolve
