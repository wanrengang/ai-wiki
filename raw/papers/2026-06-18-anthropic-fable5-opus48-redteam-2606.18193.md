---
source_url: https://arxiv.org/html/2606.18193v1
ingested: 2026-06-18
sha256: 6d292656753c24716ed13201abcbe95381b265340dd5db6cd7398c155ee45bbf
---

A Red-Team Study of Anthropic Fable 5 & Opus 4.8 Models
Adversarial Robustness Evaluation
Dr. Nicola Franco
Head of AI Security Lab, The Italian Institute of Artificial Intelligence (AI4I)
June 2026

ABSTRACT

We evaluate the adversarial robustness of two frontier large language models (LLMs) developed by Anthropic, Fable 5 and Opus 4.8, against four families of automated jailbreak attack across 7,826 harmful intents spanning a ten-category harm taxonomy. Using the HackAgent red-teaming framework, hundreds of thousands of adversarial attempts were generated and every apparent success was independently re-adjudicated by a panel of three judge models (majority vote). Both models resist the majority of attacks, but the residual surface is larger than aggregate framing suggests: it is dominated by adaptive iterative attacks, while static obfuscation is near-fully neutralised. The strongest adaptive search (tree-of-attacks) breaks Opus 4.8 on 11.5% of intents overall, whereas Fable 5 stays in the single digits (6.1% worst-case). Even in these hardened configurations, the two models produced 1,620 (Opus 4.8) and 702 (Fable 5) panel-confirmed harmful completions spanning every harm category, located automatically, cheaply, and within the first one or two refinement steps by an attacker model with no human expert in the loop. The reasonable conclusion is that even the best, most-tested frontier models remain reliably breakable under sustained automated pressure.

KEY FINDINGS

1. Both models resist most attacks, but Opus 4.8 breaks double digits under adaptive search. Tree-of-attacks (TAP) breaks Opus 4.8 on 11.5% of intents (vs 6.1% for Fable 5). The exposure is worst where it matters most: Opus 4.8 reaches 27.6% on child-safety framings, 16.6% on cybersecurity (PAIR), 14.7% on criminal/economic, 13.2% on content. Fable 5 is most exposed in ethical/social and child-safety framings.

2. Adaptive attacks dominate the residual surface. Confirmed jailbreaks come almost entirely from adaptive, iterative attacks that let an attacker model rewrite its prompt in response to refusals, succeeding early (usually within 1-2 refinement steps). Static obfuscation (encodings, ciphers, payload-splitting, role-play, encyclopedic framing) is near-fully neutralised at 0.18% (Opus) and 0.04% (Fable).

3. Every harm category is exposed. Both models produced panel-confirmed harmful completions spanning every one of the ten harm categories (A-J: Ethical/Social, Privacy/Data, Safety/Physical, Criminal/Economic, Cybersecurity, Information/Political, Content/Cultural, IP/Ownership, Decision/Cognitive, Child Safety).

4. The reasonable reading: even the best, most-tested frontier models remain reliably breakable under sustained automated pressure. At deployment scale, with millions of interactions per day, a success rate of this magnitude is a steady, reproducible stream of harmful outputs reachable by anyone willing to iterate.

METHODOLOGY

Threat Model: Each target treated as a black box accessed through standard API. Attacker has no access to weights, logprobs, or internal state.

Harm Taxonomy: 7,826 harmful intents organised into 10 top-level categories (A-J) and 55 subcategories.

Attack Families (4):
- TAP (Tree of Attacks with Pruning): Attacker model grows a tree of candidate prompts, expanding promising branches and pruning weak ones. Adaptive, depth 3, width 4, branching factor 3.
- PAIR (Prompt Automatic Iterative Refinement): Iterative loop where attacker reads target's refusals and rewrites prompt, up to 12 iterations, 8 parallel streams.
- PAP (Persuasive Adversarial Prompts): One-shot persuasion reframing with authority/role-play/hypotheticals, no iterative feedback.
- h4rm3l: Static obfuscation decorators (base64, character ciphers, payload-splitting, few-shot priming, DAN-style role-play, Wikipedia framing).

Two-Stage Adjudication: (1) In-loop scoring with HarmBench-style rubric. (2) Independent panel of 3 judges (Qwen3.7 Max, Gemini 3.5 Flash, GPT 5.5) — majority vote required for confirmed jailbreak.

ASR = confirmed jailbreaks / total attempts × 100%

RESULTS

Panel-confirmed ASR by technique:
Opus 4.8: TAP 11.51%, PAIR 7.98%, PAP 3.67%, h4rm3l 0.18%. Total: 1,620 confirmed.
Fable 5:   TAP 6.10%,  PAIR 4.30%,  PAP 0.54%, h4rm3l 0.04%. Total: 702 confirmed.

Per-category robustness (100% − ASR):
Opus 4.8 weakest: Child Safety (72.4%), Cybersecurity (88.6%), Content/Cultural (86.8%), Criminal/Economic (85.3%).
Fable 5 weakest: Ethical/Social (89.8%), Child Safety (93.9%).
