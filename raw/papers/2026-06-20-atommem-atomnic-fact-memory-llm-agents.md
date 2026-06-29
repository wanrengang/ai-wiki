---
source_url: https://arxiv.org/abs/2606.19847
ingested: 2026-06-20
sha256: 026c19c5ef4b684d6e451dc643183eb73ecec7848865cfa19c1f0560a2e0dced
---
AtomMem: Building Simple and Effective Memory System for LLM Agents via Atomic Facts
Yanyu Yao, Shangze Li, Zhi Zheng, Hui Zheng, Qi Liu, Tong Xu, Enhong Chen
1State Key Laboratory of Cognitive Intelligence, University of Science and Technology of China, Hefei, China
2Anhui University, Hefei, China

Abstract

Large language models (LLMs) demonstrate strong reasoning and generation abilities, but their fixed context windows limit long-term information accumulation and reuse across multi-session interactions. Existing memory-augmented systems often construct memory in a coarse and unstable manner, relying on inefficient memory representations or unstable unconstrained updates. To address these challenges, we propose AtomMem, a long-term memory system designed for value-dense storage and stable memory evolution. AtomMem introduces a Fact Executor, which selectively extracts high value atomic facts from long form interactions to serve as highly efficient memory representations. Subsequently, AtomMem organizes these facts into hierarchical event structures and temporal profiles, capturing coherent episodic contexts and tracking dynamically evolving user attributes over time. During retrieval, the system activates an associative memory graph to connect fragmented memories. Experiments on the LoCoMo benchmark confirm that AtomMem achieves state-of-the-art performance across various reasoning tasks, offering a scalable and economically viable solution for deploying intelligent personalized agents. The implementation code is publicly available at https://github.com/MINE-USTC/AtomMem.

1 Introduction

Large language models (LLMs) have demonstrated remarkable capabilities in language understanding, reasoning, and generation. Recent advances have extended these models into interactive agents capable of engaging in multi turn conversations spanning days or even months, requiring these agents to accumulate and organize useful memories. However, constrained by fixed-length context windows, existing models often struggle to maintain coherence and accurate retrieval over extended contexts. This often results in practical failures such as forgetting user preferences, repeating previously resolved questions, or contradicting established facts.

To address this limitation, existing memory-augmented systems like Mem0 and A-Mem incorporate graph databases or enable dynamic memory evolution. However, they face a fundamental dilemma: storing raw conversations maximizes retention but overwhelms RAG with redundant noise; condensed representations achieve compact formats but discard fine-grained details and accumulate LLM-generated noise over time. Therefore, achieving a balance between high information density and contextual fidelity is crucial.

Furthermore, user memory is inherently dynamic. Recent work explores dynamic memory evolution, but unconstrained LLM-driven updates introduce severe instability — hallucinations or erroneous edits can repeatedly modify the same memory entry, leading to uncontrolled expansion and destruction of original facts.

In this paper, we propose AtomMem, a long-term memory system centered on atomic facts that organizes user interactions into a hierarchical memory structure and activates relevant memories through graph-based associative recall.

Core components:
• Fact Executor: SFT-tuned (Qwen3-14B) Fact Extractor converts raw noisy dialogues into self-contained atomic facts via coreference resolution and temporal anchoring
• Structured Atomic Facts: F = {id, content, embedding, participants(P), keywords(K), temporal(T), associated Event IDs(E)}
• Event Memory: Groups related facts into coherent episodic narrative blocks
• Temporal Profile: Incrementally tracks stable user attributes while adapting to preference shifts
• Memory Graph: Connects facts through entity overlap, shared events, and dialogue continuity; uses Random Walk with Restart (RWR) for associative recall
• Hierarchical Retrieval: 3-stage — (1) Primary Recall (metadata-filtered hybrid similarity) → (2) Compensatory Recall (event-layer expansion) → (3) Associative Recall (graph-propagated activations) → Profile Augmentation

Main contributions:
• AtomMem framework centered on atomic facts with graph-based associative recall
• Atomic fact extraction module with SFT-trained fact extractor + high-quality dataset
• SOTA on LoCoMo benchmark; simplified AtomMem-Flat achieves competitive performance at minimal computational cost

2 Related Work

Key related systems:
• Mem0: Graph database for relational memory organization
• A-Mem: Dynamic memory evolution without predefined rules
• MemoryOS, MemGPT: Hierarchical memory management interfaces
• Think-in-Memory, RMMT: Dialogue history summarization across granularities
• RET-LLM: Memory anchored on structured triplets
• LoCoMo, LongMemEval: Long-term conversational memory benchmarks

3 Methods

3.1 Base Representation: Atomic Fact Extraction

Goal: Transform raw dialogue sessions into structured, self-contained atomic facts F.

Atomic Fact Extractor: SFT-trained on high-quality dataset D constructed via 2-stage pipeline. Fine-tuned lightweight LLM (Qwen3-14B with LoRA) compresses raw interactions while ensuring each fact is independent and comprehensible without external context.

Structured Atomic Fact F = {id, c (self-contained text), v (dense embedding), P (participants), K (keywords), T (temporal), E (associated Event IDs)}

Fact Verification: Before storage, verify new fact F_new against existing records using hybrid similarity (semantic embedding + keyword Jaccard). LLM analyzes relationship between new input and retrieved context to generate residual content (store as new fact) and update tuples (for logical conflicts). Prevents memory redundancy and maintains global consistency.

3.2 Episodic Consolidation: Event Memory Construction

Event E = {id, summary S, constituent Fact IDs F_IDS, participants P_e, keywords K_e, temporal span T_e}

Dynamically absorbs new logically aligned facts into existing events or creates new events. Transforms isolated facts into episodic memory.

3.3 Temporal Profile: User State Tracking

Temporal Profile: Captures stable user attributes and tracks preference evolution over time. Selects profile versions valid during specific time ranges to ensure responses reflect both current preferences and historical states.

3.4 Associative Memory Graph

Graph edges: entity overlap, shared events, dialogue continuity. Uses Random Walk with Restart (RWR) for associative recall. Transforms isolated facts into connected knowledge network.

3.5 Hierarchical Retrieval

3-stage retrieval:
(1) Primary Recall: Filter global facts by participant (P_q) and temporal (T_q) constraints, rank using hybrid similarity S_h, select top ks/2 facts
(2) Compensatory Recall: Expand to event layer, rank events using S_h, extract constituent facts, fuse with fusion score S_f = w_e * S_h(E,Q) + w_f * S_h(F,Q), select top ks/2 more facts
(3) Associative Recall: Use retrieved facts as seeds to activate memory graph, expand limited hops, apply RWR, select top kf facts with highest activation scores
Profile Augmentation: If I_prof=1, retrieve stable user attributes from temporal profiles

Final context construction: concatenate retrieved episodic fact set + semantic profile set → LLM → final response

4 Experiments

Dataset: LoCoMo benchmark (long-term interactions averaging 600+ turns across 35 sessions, comprehensive question sets) + LongMemEval (500 curated questions for chat assistant memory evaluation)

Baselines: LoCoMo, MemoryBank, A-MEM, MEM0, MemoryOS, LightMem (all re-implemented with GPT-4o-mini backbone)

Metrics: Token-level F1, BLEU-1, LLM-as-a-Judge (J, using deepseek-v4-pro), Token consumption

Results on LoCoMo:
| Method       | F1    | BLEU-1 | J     | Tokens(K) |
|--------------|-------|--------|-------|-----------|
| LoCoMo       | 37.81 | 26.23  | 41.67 | 827.20    |
| MemoryBank   | 17.85 | 12.13  | 23.96 | 926.98    |
| A-Mem        | 33.87 | 27.64  | 32.29 | 11687.58  |
| MEM0         | 54.95 | 44.71  | 54.17 | 55300.30  |
| MemoryOS     | 49.72 | 43.58  | 51.04 | 19207.67  |
| LightMem     | 49.30 | 42.45  | 43.75 | 5021.56   |
| AtomMem-Flat | 47.08 | 40.40  | 52.08 | 722.75    |
| AtomMem      | 56.66 | 49.56  | 64.58 | 21357.06  |

AtomMem achieves SOTA across all metrics. AtomMem-Flat (without hierarchical/event/profile/graph) achieves competitive performance at only 722K tokens vs MEM0's 55,300K — dramatic cost reduction.

Ablation study: All components contribute; graph-based associative recall and profile augmentation provide largest gains.