---
source_url: https://arxiv.org/abs/2606.23525
ingested: 2026-06-24
sha256: 67017fee1ce891a3ae7fd22d12c14262f67cbf01e11f4b49ec38f5911855e910
---

Abstract

                                                   Long agent traces composed of chains of thought and tool calls accumulate stale
                                                   content that anchor subsequent generations, and eventually outgrow the context
                                                   window. Existing scaffolds mitigate it with fixed-interval compaction triggered at a
                                                   token threshold. Such triggers pay no heed to trajectory structure, risking discard
                                                   of partial results mid-derivation or mid-search. We propose S ELF C OMPACT, a
                                                   scaffold that allows the model itself to decide when and how to compact. Specifi-
                                                   cally, it pairs two inference-time elements: (i) a compaction tool the model invokes
                                                   to summarize the accumulated context, and (ii) a lightweight rubric specifying
                                                   when to fire (a sub-task has resolved, or the trajectory is converging) and when
                                                   to suppress (mid-derivation, or when stuck). Both are needed. The tool alone is
                                                   unevenly used across open-weight models, often invoked at unhelpful moments
                                                   or not at all; the rubric alone cannot act. Together, they elicit effective adaptive
                                                   compaction without any fine-tuning or external supervision. We present empirical
                                                   results on six benchmarks (competitive math and agentic search) and seven models.
                                                   Our results show that S ELF C OMPACT matches or exceeds fixed-interval summa-
                                                   rization at a fraction of the token cost, improving over a no-summarization baseline
                                                   by up to 18.1 points on math and 5–9 points on agentic search at 30–70% lower
                                                   per-question cost. Our results expose a meta-cognitive gap: although unprompted
                                                   models cannot reliably tell when their own context is rotting, a lightweight rubric
                                                   closes this gap, reframing when to compact as a capability that scaffolds can supply
                                                   without training.


                                         1    Introduction

                                         We are chasing after harder problems over longer horizons [METR, 2026], and consequently, the
                                         trajectories LMs generate to solve them keep growing. Reasoning models are now able to spend tens
                                         of thousands of tokens deliberating on a single competition math question: Qwen3.5 produces 81k
                                         tokens [Qwen Team, 2026], Kimi-K2.5 produces 96k [Kimi Team et al., 2026]. Agentic systems
                                         extend further, orchestrating search results [Wei et al., 2025], code execution outputs [Jimenez et al.,
                                         2024], and intermediate plans [Novikov et al., 2025] across hundreds of turns. The bet that more
                                         thinking and interactions yields better answers has paid off, but long traces carry a hidden cost.
                                         As the trace grows, it accumulates junk — a flawed case split made early, a search result the model
                                         has moved past, a candidate program that led nowhere. These leftovers do not just sit there; they
                                         anchor everything that follows [Laban et al., 2026]. A model that solves a problem from a clean start
                                         often fails when fed back its own flawed reasoning. This phenomenon is known as context rot [Hong
                                         et al., 2025, Cheng et al., 2026]. Existing systems try to manage it with rigid rules: compacting the
                                         trajectory when a token threshold is met [Cursor Research et al., 2026], or delegating the burden of
                                         identifying context rot to the user via /compact [Anthropic, 2025].


                                         Preprint. Under Review.
QUESTION (BrowseComp)
 Input prompt (P): Identify a rare fungus appearing in clusters after rainfall, with raised scales on its cap, named by a French expert in the
 1980s, with potential antifungal properties. Its English name matches a 1980s film character; the film was inspired by a 1970s bronze
 statuette. Two-word name; first word has 3 syllables, ends in vowel.

 Gold answer: Medusa mushroom (Agaricus bohusii, Marcel Bon 1983; Medusa from Clash of the Titans 1981, Harryhausen 1977).

                                                                                                                               Final Answer:
      Baseline                P                                                                                      ⋯           ✗ None
   no compression
                             A few unproductive searches, then a 3 × 10K-token monologue with no further queries.


                                   poorly timed summarization — drops verified facts; restates intent vaguely.
                                                                                                                               Final Answer:
   Fixed-interval                  Agaricus ✓       Bon 1983 ✓               Harryhausen ✓
                                                                                                                                ✗ Morel
    Compression              P                                                                                       ⋯          Mushroom
 every 2 trajectories
                              Fires mid-reasoning → verified facts wiped → wasted re-searches → generic guess.


                                   adaptive summarization — consolidates facts; lets model escape from loops.
                                                                                                                               Final Answer:
   SelfCompact                    Agaricus ✓        Bon 1983 ✓       Clash 1981 ✓
                                                                                                                                ✓ Medusa
     rubric-gated
      summaries
                             P                  ✓                ✓                  ✓                                ⋯          mushroom
                            Each summary fires right after a verified fact, crystallizing it before reasoning continues.

                                       Reasoning Trajectory             Summary fire         ✓ Rubric COMPRESS



Figure 1: Comparison of trajectory-compression strategies on a hard BrowseComp question.
The gold answer requires verifying four facts (Agaricus, Bon 1983, Clash 1981, Harryhausen) before
composing Medusa mushroom. Baseline (no compression) burns its budget on an unproductive
monologue and emits no answer. Fixed-interval compression fires every two search trajectories
regardless of reasoning state; the poorly-timed summary wipes verified facts mid-reasoning, leaving
the model to guess (Morel Mushroom). S ELF C OMPACT (Ours) gates compression on a rubric over
closed reasoning units, so each summary fires immediately after a verified fact—all four facts persist
and the model retur