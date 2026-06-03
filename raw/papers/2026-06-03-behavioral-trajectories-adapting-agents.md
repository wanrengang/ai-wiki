---
source_url: https://arxiv.org/abs/2606.02536
ingested: 2026-06-03
sha256: 459cd143b59133fbccfc7572507dbf0598603290ffc7e9ca515fb11f386b63b5
---
Tracking the Behavioral Trajectories of Adapting Agents
Jonah Leshin, Manish Shah, Ian Timmis

Abstract

Text files such as skill files, memory files, and behavioral configuration files play a central role in defining how modern agents act. Through edits by humans or the agents themselves, these files may evolve over time, directly steering the agent's behavior in future interactions. We present a methodology and framework for measuring agent "traits" by defining traits as directions in the embedding space of a text embedding model. We train a linear model on labeled "before" versus "after" skill file diffs to learn a trait vector, then score arbitrary skill edits by projecting their embedding diffs onto this vector. Evaluated on 68 labeled skill diff pairs for the trait of propensity to seek sensitive data, our method achieves 91.2% sign classification accuracy and a Spearman rank correlation of ρ = 0.82 under leave-one-out cross-validation. We build this trait evaluation into a broader agent-to-agent protocol that enables one agent to evaluate another's skill file updates through a trusted intermediary.

AI agents, agent safety, trait measurement, embedding space, agent-to-agent evaluation

1 Introduction

Text files are a critical part of how modern AI agents function. Skill files define agent capabilities in systems such as OpenClaw, Hermes, and Claude Code. Memory files maintain persistent context across sessions. Behavioral configuration files such as SOUL.md (Mars, 2026) define an agent's identity, values, and constraints. These files directly shape an agent's actions: a skill file that instructs an agent to retrieve admin credentials will cause the agent to do so if it is not otherwise restricted.

This design creates a meaningful attack surface. In March 2026, Cisco researchers demonstrated that malicious instructions injected into Claude Code's memory file could silently alter agent behavior in a way that persists across sessions and projects because the agent treats the file's content as authoritative context (Cisco AI Security Research, 2026).

Tracking how these files evolve is essential since each edit can shift the agent's behavior in subsequent interactions. These files can be written and edited in multiple ways: directly by humans, via a human prompting the agent to write or update them, autonomously by the agent itself, or by some other external process with sufficient privileges. In practice, agents adapt frequently as new skills are added, existing ones are refined, and memory files accumulate context. The vast majority of these changes are benign; the challenge is to reliably flag the rare non-benign change amid a steady stream of routine updates. As agents' capabilities improve and they take on more tasks autonomously, the ability for one agent to evaluate another's behavioral properties without human intervention becomes critical. Moreover, because agents evolve, an agent that is trustworthy at one point must be reevaluated as its files change.

We make three contributions: (1) A novel methodology for measuring agent traits as directions in embedding space, applied to diffs of agent skill files over time; (2) Validation of the methodology on a "data-seeking" trait across 68 skill diff pairs, achieving 91.2% sign classification accuracy and ρ = 0.82 Spearman rank correlation under leave-one-out cross-validation; (3) An agent-to-agent protocol in which the methodology can be implemented, enabling one agent to evaluate another's text file updates through a trusted intermediary.

2 Background and Related Work

Our methodology draws on representation engineering (Zou et al., 2023), that high-level properties are encoded as linear directions in a model's activation space. We adapt this to a different setting: rather than probing internal activations, we learn trait directions in the embedding space of a text embedding model applied to agent source files.

Existing approaches: (1) Human-mediated observability platforms such as LangSmith, which provide tracing, annotation queues, and dashboards through which human reviewers assess agent behavior after execution; (2) Google's Agent2Agent (A2A) protocol, which enables agents to discover one another's capabilities by fetching a standardized JSON metadata document (an "Agent Card"). Our "agent-to-agent" protocol instead mediates evaluation of behavioral changes in source files.

3 Methodology

We focus on markdown skill files (SKILL.md) for our initial implementation. We assess the diff between "before" and "after" versions of a skill file.

3.1 Trait Vectors

We define a trait as a direction in the embedding space of a text embedding model. Given file pairs (Bi, Ai) with labels yi in [-1, 1] indicating the magnitude and direction of trait change:

1. Embed each text file with model E and normalize: ei := e/||e||.
2. Compute the diff vector di = E(Ai)^ - E(Bi)^, then normalize to obtain di^.
3. Fit a Ridge regression from di^ to yi. The resulting coefficient vector w is the trait vector.

At inference, scoring a new edit amounts to embedding the before and after files, computing the normalized diff, and taking the dot product di^ · w + b, where b is the Ridge intercept.

3.2 Model Validation

We use Ridge regression with a closed-form leave-one-out cross-validation (LOOCV) via the PRESS statistic. We evaluate with two metrics:
- Sign accuracy: fraction of diffs for which the model correctly predicts whether the edit made the skill more or less trait-positive.
- Spearman rank correlation (ρ): how well the model's continuous predictions preserve the rank ordering of the labels.

3.3 Implementation

Embedding model: Qwen3-Embedding-8B (Zhang et al., 2025), an instruction-aware text embedding model producing 4096-dimensional vectors. We prepend: "Represent this skill documentation for a security audit, focusing on whether it instructs the agent to retrieve, exfiltrate, or solicit credentials, secrets, tokens, or private user data."

Training data: 63 publicly available agent skills from the Awesome Copilot repository as "before" versions. For each, we created a synthetic "after" version that clearly increases or decreases the data-seeking trait. In 5 cases we created both versions, yielding 68 before/after pairs in total.

Labeling: Labels from -1 (strongly data-secure) to +1 (strongly data-seeking). A subset labeled by authors; remainder initially labeled by an LLM (Claude Opus 4.6), then manually reviewed.

3.4 Results

Under leave-one-out cross-validation on the 68-pair dataset, the model correctly classified whether skill edits made a skill more or less data-seeking 91.2% of the time, with a Spearman rank correlation of ρ = 0.82.

Of the 68 pairs, 6 are misclassified by sign. All 6 misclassified predictions have small absolute values (mean |y^| = 0.085), indicating the model is uncertain rather than confidently wrong.

Baseline comparison:
- YARA signature baseline: 63.2% sign accuracy over all 68 diffs
- Frontier LLM (GPT-5.4): 100% sign accuracy
Our method is between signature-based rules and a frontier LLM: substantially improves over YARA-style baseline while retaining deterministic, fast, and auditable scoring.

4 Agent-to-Agent Protocol

Protocol involves three actors:
- Agent A (requester): wants to evaluate Agent B's text files for a given trait.
- Agent B (executor): contains the text files being evaluated.
- Runtime server (intermediary): trusted third party that mediates the evaluation.

Protocol steps:
1. Agent A requests a trait evaluation of Agent B via the runtime server.
2. Agent B polls the server for task requests, accepts the task, and receives a containerized executable (embedding model).
3. Agent B runs the executable locally on its before/after skill files and submits the raw diff vectors to the runtime server.
4. The runtime server runs a processor, applies the trait vector, computes the scalar trait score, and returns the result to Agent A.

Key properties: agents are pure HTTP clients (neither exposes inbound endpoints); execution is separated from server-side scoring.

4.1 Trust and Continuity

Hash chaining: Agent B includes a hash of each before and after file, and Agent A references the previous "after" hash in its next request.

Threat model: A malicious Agent A cannot directly inspect Agent B's files or choose arbitrary evaluation code. A malicious Agent B cannot simply report a favorable score, since the server controls the trait vector and scoring processor.

4.2 Deployment

Deployed with a Hermes (Nous Research, 2026) agent acting as both Agent A and Agent B. A Hermes cron job checks for skill files updated since the last successful evaluation. When updates are found, the agent initiates the full protocol flow.

5 Limitations and Future Work

We validate one trait, data-seeking, on 63 public "before" skills from one repository. Extending to more traits, more diverse deployed-agent skills, and correlations between traits are left to future work.