---
source_url: https://arxiv.org/abs/2605.30335
ingested: 2026-05-30
sha256: 9f21fe0fb8fa07df017b97e704161cd13be6e5b8900fffcd185a7ba85627dc70
---
Locally Coherent, Globally Incoherent: Bounding Compositional Incoherence in Multi-Component LLM Agents

Anany Kotawala

Abstract

Multi-component LLM agents assemble probabilistic claims from components that each see only part of a joint problem; the composition can violate basic probability axioms even when every component is locally coherent. We formalise this locally coherent, globally incoherent failure via the compositional residual ε⋆, the L2 distance from the composed quote to the joint coherent polytope, computable at runtime from system output and the declared cross-component coupling constraints. A product-structure dichotomy characterises when local coherence suffices, and a Rayleigh-quotient prediction matches the observed residual within 7% on three of four relation classes. A hierarchical Boyle–Dykstra projection repairs the composition deterministically; an anytime-valid e-process gives sequential coherence monitoring. Across 1,876 ensemble cliques on a four-LLM mid-tier panel, ε⋆ > 0 on 33–94% of cliques, translating to +0.115 nats per bet of regret on 1,770 resolved bets under the proportional allocation rule. Three intuitive LLM-side mitigations (retrieval, partition-aware prompting, aggregator-LLM) each fail or regress.

foundation models, compositional generalization, formal performance guarantees, decomposable benchmark, instance-wise uncertainty, calibration, hierarchical projection, FTAP, anytime-valid inference, Brier score, multi-component systems

1 Introduction

Foundation-model evaluation routinely reports per-question accuracy, calibration, and proper-scoring statistics; formal, instance-wise guarantees on system-level performance under composition of multiple model calls are comparatively scarce. In multi-component agents, planners route retrieval, arithmetic, and probability assessment to specialist subagents or tools, and each component handles only part of a joint question. Even when every component is calibrated and internally coherent on its assigned questions, the aggregated belief need not be: a research component emitting P(Republican)=0.6 and a forecasting component emitting P(Democrat)=0.6 produce a 1.2-mass quote that no probability measure can assign, inducing a Dutch-book exposure (de Finetti, 1937) that arises strictly between components. Per-component coherence does not, in general, repair the composed system.

We identify the compositional residual ε⋆, the L2 distance from the composed quote to the joint coherent polytope, as a runtime, distribution-free certificate of system-level coherence under owner-selected aggregation. A product-structure dichotomy (Theorem 3.3) characterises when local coherence suffices: under owner-selected aggregation, component coherence guarantees system coherence for all inputs if and only if the joint polytope factorises as a Cartesian product of local polytopes. Under any tighter coupling, locally coherent component forecasts exist whose composition is globally incoherent.

Operationally, ε⋆ yields a bound Exposure⋆ ≤ m⋆ ε⋆ on system-level Dutch-book exposure under the Fundamental Theorem of Asset Pricing (FTAP); the hierarchical Boyle–Dykstra repair drives this bound to the numerical floor. The e-process gives an anytime-valid coherence test.

Empirically, we evaluate 1,876 ensemble cliques drawn from Paleka and Polymarket across four contemporary foundation models, stratified by four logical-relation classes. The empirical hardness ordering (partition > negation > disjunction > conjunction) tracks how tightly each polytope constrains the joint quote. Rayleigh-quotient magnitude prediction matches observed residual within 7% on three of four relation classes.