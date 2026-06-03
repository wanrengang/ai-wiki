---
source_url: https://arxiv.org/html/2605.22791v1
ingested: 2026-05-24
sha256: a5e6e9e4daac60e3eda38fa04623239d64e93abfd6ace6ede7113ed139f5dc24
---

Gated DeltaNet-2: Decoupling Erase and Write in Linear Attention
Authors: Ali Hatamizadeh, Yejin Choi, Jan Kautz (NVIDIA)
arXiv: 2605.22791v1 [cs.AI] 21 May 2026

ABSTRACT
Linear attention replaces the unbounded cache of softmax attention with a fixed-size recurrent state, reducing sequence mixing to linear time and decoding to constant memory. The hard part is not just what to forget, but how to edit this compressed memory without scrambling existing associations. Delta-rule models subtract the current read before writing a new value, and Kimi Delta Attention (KDA) sharpens forgetting with channel-wise decay. But the active edit still uses a single scalar gate to control two different things, how much old content to erase on the key side and how much new content to commit on the value side. We introduce Gated DeltaNet-2, which generalizes both Gated DeltaNet and KDA by inheriting adaptive forgetting and channel-wise decay while addressing their shared limitation, the scalar tie between erasing and writing. Gated Delta Rule-2 separates these roles with a channel-wise erase gate and a channel-wise write gate, reducing to KDA when both gates collapse to the same scalar and to Gated DeltaNet when the decay also collapses. We derive a fast-weight update view, a chunkwise WY algorithm with channel-wise decay absorbed into asymmetric erase factors, and a gate-aware backward pass that preserves efficient parallel training. At 1.3B parameters trained on 100B FineWeb-Edu tokens, Gated DeltaNet-2 achieves the strongest overall results among Mamba-2, Gated DeltaNet, KDA, and Mamba-3 variants across language modeling, commonsense reasoning, and retrieval. Its advantage is most pronounced on long-context RULER needle-in-a-haystack benchmarks, where it improves the evaluated multi-key retrieval setting and remains strong in both recurrent and hybrid settings.

KEY CONTRIBUTIONS

1. Decoupled Erase and Write Gates
- KDA uses a single scalar gate β_t to control both erasing old content (key side) and writing new content (value side)
- Gated DeltaNet-2 introduces channel-wise erase gate b_t (key axis) and channel-wise write gate w_t (value axis)
- Erasing: decide which coordinates of old read should be removed
- Writing: decide which coordinates of incoming value should be committed

2. Fast-Weight Update Interpretation
- Gated Delta Rule-2 is the solution of a local online optimization problem
- First decayed state S̄_t = D_t × S_{t-1}
- Then read r_t = S̄_t^T × e_t (gated erase direction)
- Write: S_t = S̄_t + k_t × (z_t - r_t)^T where z_t = w_t ⊙ v_t
- Preserves efficient parallel training via gate-aware backward pass

3. Chunkwise WY Algorithm
- Absorbs cumulative channel-wise decay into rank-one erase factors
- Same high-level chunkwise structure as efficient delta-rule kernels
- Enables efficient parallel training

EXPERIMENTAL RESULTS

- 1.3B parameters trained on 100B FineWeb-Edu tokens
- Outperforms Mamba-2, Gated DeltaNet, KDA, and Mamba-3 variants
- Best on: language modeling, commonsense reasoning, retrieval
- Most pronounced advantage: long-context RULER needle-in-a-haystack benchmarks
- Especially effective on multi-key retrieval (fixed-size state separating competing associations)
- Strong in both recurrent and hybrid settings

RELATIONSHIP TO PRIOR WORK

| Method | Erase | Write | Decay |
|--------|-------|-------|-------|
| DeltaNet | scalar | scalar | none |
| Mamba-2 | scalar | full | scalar |
| Gated DeltaNet | scalar | scalar | scalar |
| KDA | scalar | scalar | channel-wise |
| Gated DeltaNet-2 | channel-wise | channel-wise | channel-wise |

Code: https://github.com/NVlabs/GatedDeltaNet-2
