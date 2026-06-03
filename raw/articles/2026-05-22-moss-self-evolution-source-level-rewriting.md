---
source_url: https://arxiv.org/abs/2605.22794
ingested: 2026-05-22
sha256: d6f8f0769b0961fd19e30288479d5f43de648ff6cbc7719eae0c489c4687d17b
---

# MOSS: Self-Evolution through Source-Level Rewriting in Autonomous Agent Systems

**arXiv:** 2605.22794
**Submitted:** 21 May 2026
**Authors:** Qianshu Cai, Yonggang Zhang, Xianzhang Jia, Wei Xue, Jun Song, Xinmei Tian, Yike Guo

## Abstract

Autonomous agentic systems are largely static after deployment: they do not learn from user interactions, and recurring failures persist until the next human-driven update ships a fix. Self-evolving agents have emerged in response, but all confine evolution to text-mutable artifacts -- skill files, prompt configurations, memory schemas, workflow graphs -- and leave the agent harness untouched. Since routing, hook ordering, state invariants, and dispatch live in code rather than in any text artifact, an entire class of structural failure is physically unreachable from the text layer.

We argue that source-level adaptation is a fundamentally more general medium: it is Turing-complete, a strict superset of every text-mutable scope, takes effect deterministically rather than through base-model compliance, and does not erode under long-context drift.

We present **MOSS**, a system that performs self-rewriting at the source level on production agentic substrates. Each evolution is anchored to an automatically curated batch of production-failure evidence and proceeds through a deterministic multi-stage pipeline; code modification is delegated to a pluggable external coding-agent CLI while MOSS retains stage ordering and verdicts. Candidates are verified by replaying the batch against the candidate image in ephemeral trial workers, then promoted via user-consent-gated, in-place container swap with health-probe-gated rollback.

**On OpenClaw, MOSS lifts a four-task mean grader score from 0.25 to 0.61 in a single cycle without human intervention.**

## Key Contributions

1. **Source-level adaptation**: First system to extend self-evolution beyond text-mutable artifacts to actual source code
2. **Deterministic pipeline**: Multi-stage pipeline with production-failure evidence anchoring
3. **Verified deployment**: Ephemeral trial workers + health-probe rollback + user-consent gating
4. **Pluggable coding agent**: Code modification delegated to external CLI (e.g., Claude Code, OpenCode)

## Architecture

- **Input**: Production failure evidence batch (automatically curated)
- **Pipeline stages**: Evidence curation → Code modification (external agent) → Stage verdict → Candidate promotion
- **Verification**: Replay batch against candidate in ephemeral trial workers
- **Deployment**: User-consent-gated in-place container swap with health-probe rollback

## OpenClaw Integration

MOSS was evaluated on OpenClaw, lifting the four-task mean grader score from 0.25 to 0.61 in a single evolution cycle. The external coding-agent CLI is pluggable -- could use Claude Code, OpenCode, or similar.

## Relationship to Library Drift (Ratchet)

MOSS addresses source-level rewriting while Ratchet (arXiv:2605.19576) addresses skill library lifecycle management. Together they cover two failure modes of self-evolving agents:
- MOSS: structural failures unreachable from text layer
- Ratchet: unbounded skill accumulation without outcome-driven lifecycle

## Source

- arXiv: https://arxiv.org/abs/2605.22794
- Full paper HTML: https://arxiv.org/html/2605.22794
