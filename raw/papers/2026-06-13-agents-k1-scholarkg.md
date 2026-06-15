---
source_url: https://arxiv.org/html/2606.13669v1
ingested: 2026-06-13
sha256: 8afafe604da6844225af92be4e664bdc7f3acc8f68f7bb43af4da8746267e190
---
# Agents-K1: Towards Agent-native Knowledge Orchestration

Zongsheng Cao, Bihao Zhan, Jinxin Shi, Jiong Wang, Fangchen Yu, Zhijie Zhong, Zijie Guo, Tianshuo Peng, Zhuo Liu, Yi Xie, Xiang Zhuang, Yue Fan, Runmin Ma, Shiyang Feng, Xiangchao Yan, Anran Liu, Peng Ye, Wenlong Zhang, Shufei Zhang, Chunfeng Song, Fenghua Ling, Jie Zhou, Liang He, Bo Zhang, Lei Bai

Shanghai AI Laboratory

## Abstract

Current LLM-based research agents have advanced through agent orchestration, yet largely overlook scientific knowledge orchestration. Existing works often reduce papers to abstracts, surface mentions, and flat "cites" edges, omitting key entities, claims, evidence, mechanisms, and method lineages essential for scientific reasoning. To this end, we introduce Agents-K1, an end-to-end knowledge orchestration pipeline that converts raw documents into agent-native scientific knowledge graphs. Agents-K1 integrates three components: (1) a multimodal parser whose five-module schema captures entities, multimodal evidence, citations, and typed inter-entity relations across the full paper rather than abstracts alone; (2) a 4B information-extraction backbone trained with GRPO under a rule-based reward; and (3) a graphanything CLI, a tri-source agent interface that unifies web search, multimodal graph retrieval, and cross-document traversal. On top of this, we process 2.46 million scientific papers across six subjects to produce Scholar-KG, releasing a one-million-paper subset.

## Core Contributions

### 1. Five-Module Multimodal Schema
Captures across the full paper (not just abstract):
- Metadata (title, authors, venue, year)
- Explicit mentions (entities, facts)
- Implicit abstractions (claims, mechanisms, method links)
- Citation intent (extends, challenges, cites baseline)
- Fine-grained inter-entity relations

### 2. GRPO-trained 4B Extraction Backbone
- Group Relative Policy Optimization with rule-based reward
- Jointly supervises: format compliance, JSON validity, task-conditioned F1
- Tasks: NER, relation extraction, long-form structured extraction
- Surpasses 8B open-source reference across 10 benchmarks; matches 32B base on NER
- Inexpensive to retrain on new domains

### 3. GraphAnything CLI — Tri-Source Agent Interface
Unifies three sources in one interface:
- Real-time web search
- Multimodal knowledge graph retrieval
- Cross-document network traversal

Closed-loop workflow: idea generation → method specification → code synthesis

### 4. Scholar-KG — 2.46M Paper Knowledge Graph
- 6 disciplines: CS, chemistry, biology, earth science, physics, materials
- Releases 1M-paper subset
- Schema-adaptive variant can build General-KG over arbitrary corpora

## Key Results

### FrontierScience-Research Benchmark
| Model | Overall Accuracy |
|-------|-----------------|
| Gemini-3 (baseline) | 7.9% |
| Gemini-3 + Agents-K1 | 24.6% |
| GPT-5.2 (baseline) | 25.2% |
| GPT-5.2 + Agents-K1 | 39.4% |

### Multi-hop QA (HotpotQA, 2WikiMultiHopQA, MuSiQue)
State-of-the-art against 9 graph-augmented retrieval baselines

## Related Concepts
- [[harness-engineering]] — Agents-K1 follows a "harness" philosophy for agentic workflows
- [[test-time-compute-scaling]] — Both address LLM reasoning limitations via architectural innovation
- [[ragflow]] — Scholar-KG is a graph-native RAG alternative for scientific literature
- [[mem0]] — Knowledge graph as persistent memory for research agents