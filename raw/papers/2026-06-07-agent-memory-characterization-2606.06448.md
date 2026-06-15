---
source_url: https://arxiv.org/abs/2606.06448
ingested: 2026-06-07
sha256: a40eff8404b1efa9dd93be89ce0e121e54fa62739e6d8f05d2a3a29d0a89845a
---
Agent Memory: Characterization and System Implications of Stateful Long-Horizon Workloads
arXiv:2606.06448v1 [cs.AI] 04 Jun 2026

Authors: Yasmine Omri (Stanford), Ziyu Gan, Zachary Broveak, Robin Geens, Zexue He, Alex Pentland, Marian Verhelst, Tsachy Weissman, Thierry Tambe

Abstract
LLM agents are increasingly deployed on long-horizon tasks requiring sustained reasoning over extended interaction histories. Realizing this at scale requires agents to persistently store, retrieve, and update their own memory across sessions. A rich ecosystem of agent memory systems has emerged spanning flat retrieval, LLM-mediated extraction, consolidating fact stores, and agentic control flows. Yet, their system-level behavior remains uncharacterized. We present the first systems characterization of agent memory. First, we introduce a system-oriented taxonomy classifying agent memory systems along four axes. Second, we build a phase-aware profiling harness attributing cost to construction, retrieval, and generation. Third, we characterize ten representative systems across two benchmark suites, uncovering how design choices shift cost across the write and read paths. Finally, we derive 10 system recommendations covering construction scheduling, capability floors, amortization via query volume, freshness-latency tradeoffs, and fleet-scale management.

1 Introduction
LLM agents are increasingly deployed as autonomous agents capable of multi-step reasoning, tool use, and persistent interaction over extended time horizons. Agent memory generalizes RAG from static retrieval to mutable state management. The memory corpus is no longer a fixed document collection but a mutable state produced by the agent's own interaction stream, often per user, and may be appended, summarized, consolidated, linked, or rewritten across sessions as new evidence arrives.

Three fundamental limits of context-only approaches:
1. Multi-session interactions exceed any fixed context budget — external memory is a functional prerequisite
2. Prefill costs scale quadratically with history length; prefix caching fails across sessions
3. Reasoning and recall fidelity degrade significantly in long sequences (U-shaped curve: facts in the middle are routinely lost)

2 Agent Memory Paradigms

Agent Memory Execution Pipeline (7 stages): ingestion → memory construction → storage → retrieval → prompt assembly → generation → maintenance

Four paradigms (taxonomy):

PARADIGM I: Long-context memory
- long_context: no construction, raw context in prompt, append-only, passthrough retrieval

PARADIGM II: Flat RAG memory
- BM25: chunk interaction history into inverted index, no LLM, lexical top-k retrieval, append-only
- EmbedRAG: chunk and embed into dense index, single-shot append-only, both paths route through embedding service

PARADIGM III.a: Structure-augmented RAG (append-only)
- GraphRAG: LLM extracts entity-relation structure from chunks, graph store + dense, graph expand retrieval
- HippoRAG v2: OpenIE-style extraction, multi-view graph (passages/entities/facts), PPR reranking

PARADIGM III.b: Structure-augmented RAG (consolidating)
- Mem0: rewrites conversation turns into atomic facts, resolves against existing memories, fact top-k retrieval, consolidates
- SimpleMem: hybrid store, iterative hybrid retrieval, consolidates

PARADIGM IV: Agentic control flow
- A-Mem: graph store, graph-structured map-reduce, LLM-controlled writes and mutation
- Letta: blocks+archive storage, tool calls for retrieval, full mutation
- MIRIX: multi-store DB, routed typed-memory search, full mutation

Key finding: Design choices drastically redistribute cost asymmetrically — a system that compresses history into atomic facts slashes query-time prompt length but pays orders-of-magnitude more in construction-time LLM prefill.

3 Workload Suite and Profiling Harness
Phase-aware profiling harness tracking: tokens, model calls, GPU utilization, latency across construction, retrieval, and generation.

4 Characterizing Agent Memory Workloads
Findings: Construction cost shape, capability thresholds, amortization structure, freshness scheduling, footprint growth, retrieval tail behavior.

10 System Recommendations (derived):
1. Construction scheduling optimization
2. Capability floors for memory systems
3. Amortization via query volume
4. Freshness-latency tradeoffs
5. Fleet-scale management
(and 5 more from the paper)
