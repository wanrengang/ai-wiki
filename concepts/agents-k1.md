---
title: Agents-K1
created: 2026-06-13
updated: 2026-06-13
type: entity
tags: [research, agent, knowledge-graph, scientific-ai, extraction, shanghai-ai-lab]
sources: [raw/papers/2026-06-13-agents-k1-scholarkg.md]
confidence: high
---

# Agents-K1

Agent-native knowledge orchestration pipeline from Shanghai AI Laboratory (arXiv:2606.13669v1). Converts raw scientific documents into structured knowledge graphs purpose-built for research agent reasoning.

## Overview

**Problem:** LLM research agents handle agent orchestration well but neglect scientific knowledge orchestration. Existing systems reduce papers to abstracts and flat citation edges, losing key entities, claims, evidence, mechanisms, and method lineages essential for scientific reasoning.

**Solution:** Agents-K1 is an end-to-end pipeline with three integrated components that convert raw documents into agent-native multimodal knowledge graphs at scale.

## Architecture — Three Layers

### 1. KG Layer: Multimodal Parser (Five-Module Schema)
Full-paper parsing (not just abstract) via MinerU-based offline parser:
- **Metadata** — title, authors, venue, year
- **Explicit mentions** — entities and facts
- **Implicit abstractions** — claims, mechanisms, method links
- **Citation intent** — extends, challenges, cites baseline
- **Fine-grained inter-entity relations**

Figures, tables, and equations are first-class evidence alongside textual entities.

### 2. LLM Layer: GRPO-trained 4B Extraction Backbone
- Trained with Group Relative Policy Optimization (GRPO) + rule-based reward
- Jointly supervises: format compliance, JSON validity, task-conditioned F1
- Tasks: NER, relation extraction, long-form structured extraction
- **Surpasses 8B open-source reference across 10 benchmarks**
- Matches 32B base on NER while being inexpensive to retrain

### 3. CLI Layer: GraphAnything
Tri-source agent interface unifying:
- Real-time web search
- Multimodal knowledge graph retrieval
- Cross-document network traversal

Closed-loop workflow: idea generation → method specification → code synthesis. Exposes graph via CLI/MCP/API.

## Scholar-KG — 2.46M Paper Knowledge Graph

- 6 disciplines: CS, chemistry, biology, earth science, physics, materials
- **Releases 1M-paper subset** to community
- Schema-adaptive variant can build General-KG over arbitrary corpora (general-domain data synthesis)

## Benchmarks

### FrontierScience-Research (Primary)
| Model | Overall Accuracy |
|-------|-----------------|
| Gemini-3 baseline | 7.9% |
| Gemini-3 + Agents-K1 | **24.6%** (+16.7pp) |
| GPT-5.2 baseline | 25.2% |
| GPT-5.2 + Agents-K1 | **39.4%** (+14.2pp) |

### Multi-hop QA (HotpotQA, 2WikiMultiHopQA, MuSiQue)
State-of-the-art against 9 graph-augmented retrieval baselines.

## Relationships

- [[ragflow]] — Scholar-KG is a graph-native RAG alternative for scientific literature
- [[mem0]] — Both provide persistent knowledge memory for agents; Scholar-KG targets scientific papers specifically
- [[harness-engineering]] — Agents-K1 follows a "harness" philosophy for agentic research workflows
- [[eurekagent]] — Both address Agent Environment Engineering for autonomous scientific discovery
- [[test-time-compute-scaling]] — Both address LLM reasoning limitations via architectural innovation

## Tags
- #research #agent #knowledge-graph #scientific-ai #extraction #shanghai-ai-lab