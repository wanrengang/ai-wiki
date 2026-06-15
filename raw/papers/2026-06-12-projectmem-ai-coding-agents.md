---
source_url: https://arxiv.org/abs/2606.12329
ingested: 2026-06-12
sha256: 8837b79b31e2f9092406a95e98f3d87367dce518147dc7baa73105348a10f692
---

PROJECTMEM: A Local-First, Event-Sourced Memory and Judgment Layer for AI Coding Agents

Ripon Chandra Malo, Tong Qiu
University of Utah
(June 2026)

Abstract

AI coding assistants now support a growing share of software work, from quick scripts to production applications. Yet these agents remain largely "stateless": each new session re-reads project files, re-derives prior decisions, and—most costly—may repeat debugging attempts that already failed. Reconstructing this context can consume an estimated 5,000–20,000 tokens per session; the bottleneck is often not model capability but missing project memory. We present projectmem, an open-source, local-first memory and judgment layer for AI coding agents. projectmem records development as an append-only, plain-text event log of typed events—issues, attempts, fixes, decisions, and notes—and deterministically projects that log into compact, AI-readable summaries served through the Model Context Protocol (MCP). Beyond storage, projectmem adds a deterministic pre-action gate that warns an agent before it repeats a previously failed fix or edits a known-fragile file. We frame this as "Memory-as-Governance": memory that does not merely answer the agent but acts on its next action. The system runs fully offline with no telemetry; its immutable log also serves as a provenance trail for reproducible, auditable AI-assisted development. projectmem ships as a three-dependency Python package (14 MCP tools, 19 CLI commands, 37 automated tests) and is evaluated through a two-month self-study across 10 projects comprising 207 logged events. Source code: https://github.com/riponcm/projectmem.

1 Introduction

Large-language-model (LLM) coding agents have rapidly become everyday development infrastructure. These agents are powerful within a single session but stateless across sessions. When the conversation window closes, the agent loses durable project-specific state. The next session often begins by re-reading source files, re-asking questions answered yesterday, re-deriving architectural decisions, and—most costly—re-attempting fixes that have already been tried and have already failed.

This is not a model-quality problem; it is an architecture problem. Re-establishing context by re-reading a project consumes thousands of tokens per session. The cheapest moment to stop a repetition is before it happens—not after.

Contributions:
1. An event-sourced, plain-text memory substrate: an append-only log of typed events (issue/attempt/fix/decision/note) from which a compact, AI-readable summary is deterministically projected. No vector database, no embeddings.
2. A judgment layer: a deterministic, history-derived pre-action gate that warns an agent before it repeats a previously-failed fix or edits a file with a record of churn or open issues. We name this Memory-as-Governance.
3. A local-first, tool-agnostic system: a native MCP server (14 typed tools) that serves identical memory to multiple MCP-capable clients, plus a universal Markdown bridge for non-MCP tools—running fully offline with default-on secret redaction.
4. An open-source implementation: a three-dependency Python package with 37 automated tests, evaluated through a 207-event, 10-project self-study.

2 Related Work

Memory-as-Tool systems (MemGPT, Mem0, Zep/Graphiti, A-MEM, MemMachine, Memanto): one query in, one flat top-k list of passages out. These are embedding- or graph-backed, conversational or personalization-oriented, frequently cloud-hosted, typically mutable, and evaluated on QA benchmarks. Crucially, all augment context rather than gate an action.

Codified Context: independently adopts a plain-text, file-based, no-vector-database design served over MCP, motivated by the identical observation that agents "lose coherence across sessions, forget project conventions, and repeat known mistakes." Decisive difference: Codified Context prevents repetition passively—it supplies conventions as context the agent must read. projectmem instead derives a deterministic, per-action warning from an immutable, append-only event log of typed failures.

Reflexion: stores verbal feedback on a failed trial in an episodic buffer that improves the next trial without weight updates. These are retrospective. projectmem is Reflexion's episodic memory externalized—made persistent across sessions and projects, structured into a fixed typed schema, and converted from a post-hoc next-trial hint into a pre-action gate.

AGrail: a boolean pre-action gate that blocks an agent's action, with a memory module that iteratively optimizes its safety checks over a lifetime. ToolSafe performs proactive step-level pre-execution reasoning. LlamaFirewall layers jailbreak, alignment, and insecure-code checks at runtime. These share projectmem's timing—intervening before the action—but differ in mechanism: their gates are LLM- or RL-trained and therefore non-deterministic, and they target generic safety categories, not a particular project's own recorded failed fixes.

3 System Design

Event Schema: Five event types—Issue, Attempt, Fix, Decision, Note—each with a required timestamp, optional file-path, optional tag, and optional free-text body.

Projection: The event log is deterministically projected into a compact, AI-readable summary. The projection is a plain-text Markdown document, grep-able, diff-able, and git-native. No LLM call required.

Judgment Gate: Before an agent attempts a fix, it queries the judgment gate with the proposed action. The gate checks: (1) Has this fix been attempted before? (2) Does the target file have a history of churn or open issues? If either is true, the gate returns a warning before the action executes. This is the Memory-as-Governance mechanism.

4 Architecture

Native MCP server with 14 typed tools:
- projectmem_log_event: record typed event
- projectmem_query_judgment: check before action
- projectmem_get_summary: retrieve projected summary
- projectmem_list_events: list events by type/date
- projectmem_sync: sync log across projects
Plus universal Markdown bridge for non-MCP tools.

5 Operational Capabilities

Cross-session memory: agent's memory persists across sessions without re-reading files.

Pre-action governance: deterministic warning before repeated failed fix or fragile file edit.

Provenance trail: immutable log serves as audit trail for reproducible AI-assisted development.

Offline-first: no telemetry, no network required.

Secret redaction: sensitive values (API keys, passwords) redacted by default.

6 Implementation

Three-dependency Python package. 14 MCP tools. 19 CLI commands. 37 automated tests. Available at https://github.com/riponcm/projectmem.

7 Evaluation

Two-month self-study across 10 projects comprising 207 logged events. Estimated token-cost analysis: re-establishing context costs 5,000–20,000 tokens per session; projectmem's read cost is roughly fixed after a one-time write.

Key finding: agents repeat known-failed fixes across sessions; projectmem's pre-action gate catches these before they happen.

8 Limitations

- Single-user evaluation (no multi-user study)
- Pre-action gate is binary (warn) rather than adaptive (suggest alternative)
- File-level judgment does not capture fine-grained line-level churn history
- Event schema is fixed; extensibility not yet implemented

9 Conclusion

projectmem addresses the cross-session memory problem for AI coding agents through an event-sourced, plain-text, local-first memory substrate and a deterministic pre-action judgment gate. Memory-as-Governance moves beyond answering the agent to acting on its next action. The system is fully offline, open-source, and tool-agnostic via MCP.
