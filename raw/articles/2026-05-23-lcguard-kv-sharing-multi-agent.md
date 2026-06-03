---
source_url: https://arxiv.org/abs/2605.22786
ingested: 2026-05-23
sha256: eb831c0ec963c849143c2621071dbfcedf41e47c6ce8c837d6b49b1c1141b8b9
---

LCGuard: Latent Communication Guard for Safe KV Sharing in Multi-Agent Systems

Authors: Sadia Asif, Mohammad Mohammadi Amiri (Rensselaer Polytechnic Institute), Momin Abbas, Prasanna Sattigeri, Karthikeyan Natesan Ramamurthy (IBM Research)
arXiv: https://arxiv.org/abs/2605.22786
Date: 2026-05-21

Abstract:
Large language model (LLM)-based multi-agent systems increasingly rely on intermediate communication to coordinate complex tasks. While most existing systems communicate through natural language, recent work shows that latent communication, particularly through transformer key-value (KV) caches, can improve efficiency and preserve richer task-relevant information. However, KV caches also encode contextual inputs, intermediate reasoning states, and agent-specific information, creating an opaque channel through which sensitive content may propagate across agents without explicit textual disclosure. To address this, we introduce LCGuard (Latent Communication Guard), a framework for safe KV-based latent communication in multi-agent LLM systems. LCGuard treats shared KV caches as latent working memory and learns representation-level transformations before cache artifacts are transmitted across agents. We formalize representation-level sensitive information leakage operationally through reconstruction: a shared cache artifact is unsafe if an adversarial decoder can recover agent-specific sensitive inputs from it. This leads to an adversarial training formulation in which the adversary learns to reconstruct sensitive inputs, while LCGuard learns transformations that preserve task-relevant semantics and reduce reconstructable information. Empirical evaluations across multiple model families and multi-agent benchmarks show that LCGuard consistently reduces reconstruction-based leakage and attack success rates while maintaining competitive task performance compared to standard KV-sharing baselines.

Key Concepts:
- Multi-Agent LLM Systems: Multiple specialized agents collaborate through coordination, delegation, and intermediate information exchange
- Latent Communication: Instead of text-based communication (serialize internal state to natural language), agents directly transfer KV caches and intermediate activations
- KV Caches as Shared Working Memory: KV caches treated as shared working memory enabling tighter coupling between agents
- Threat Model: KV caches encode contextual inputs, intermediate reasoning states, and attention structure - information that never appears in textual outputs can still be propagated
- LCGuard Approach: Representation-level transformations before KV cache transmission; adversarial training formulation where adversary learns to reconstruct sensitive inputs while LCGuard learns to preserve task semantics while reducing reconstructable information
- Operational Definition of Leakage: A shared cache artifact is unsafe if an adversarial decoder can recover agent-specific sensitive inputs from it

Core Contributions:
1. LCGuard framework for safe KV-based latent communication
2. Formalization of representation-level sensitive information leakage through reconstruction
3. Adversarial training formulation with adversary vs LCGuard game
4. Empirical evaluation across multiple model families and multi-agent benchmarks
5. Maintains competitive task performance while reducing leakage

Related Work References:
- Li et al. 2023: Multi-agent LLM coordination
- Wu et al. 2024: Multi-agent systems
- Shi et al. 2026: Latent communication between agents
- Fu et al. 2026: KV caches as communication substrates
- Oomerjee et al. 2026: Internal representations retaining input information
