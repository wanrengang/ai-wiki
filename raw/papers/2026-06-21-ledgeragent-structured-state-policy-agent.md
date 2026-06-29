---
source_url: https://arxiv.org/abs/2606.20529
ingested: 2026-06-21
sha256: 1a8a642c33a320f06374bb0321b8115eca96a9a6dc3fc2cf217aa1fa08bf2831
---

# LedgerAgent: Structured State for Policy-Adherent Tool-Calling Agents

Md Nayem Uddin, Amir Saeidi, Eduardo Blanco, Chitta Baral
Arizona State University / University of Arizona

Abstract

Policy-adherent tool-calling agents in customer-service domains must maintain task states across turns while calling tools and obeying domain policies. Task states consist of relevant facts, identifiers, constraints, and conditions observed through user interaction and tool calls. In standard agents, task states are not represented separately. Observations, tool returns, and policy instructions are placed in the prompt, leaving agents to reconstruct the relevant states from the prompt each time they decide what to do next. This design makes state management implicit, creating two common failure modes. An agent may retrieve the right facts but later ground its decision in stale, missing, or incorrect information; and a syntactically valid tool call may still violate a domain policy that depends on the current task state. We introduce LedgerAgent, an inference-time method for tool-calling agents that maintains observed task states in a separate ledger and renders the states into the prompt. The ledger is also used to check state-dependent policy constraints before environment-changing tool calls are executed, blocking policy violations. Across four customer-service domains and a mixed panel of open- and closed-weight models, LedgerAgent improves average pass^k over a standard prompt-based tool-calling approach, with the largest gains under stricter multi-trial consistency metrics.

1 Introduction

Language agents are increasingly evaluated in settings that require sustained interaction rather than isolated tool calls. They must converse with users, retrieve records from external systems, and follow domain-specific rules across multiple turns. Customer-service domains make this requirement concrete: an agent may inspect a reservation, check an order, change a service plan, issue a refund, or update an account. Success therefore depends on more than selecting the right tool. The agent must maintain the relevant interaction state and act only when the domain policy permits the action.

Most tool-calling agents expose information to the model through prompt text. Tool outputs are appended to the prompt. Prior actions remain interleaved with user messages and model generations. The policy document is supplied as natural-language instructions. At each turn, the model must identify which prior facts matter, decide whether more information is needed, choose the next response or tool call, and judge whether the intended action is allowed. This design is simple and model-agnostic. However, it leaves task state implicit in an ever-growing context, so reliable behavior depends on finding and using the relevant evidence when the agent acts in the environment.

A second failure appears at the policy boundary. Domain policies specify when actions are allowed. These rules are usually supplied before the agent has retrieved the records that determine which rules apply. When the agent later proposes an action, there is often no separate check against the current state and governing policy. A tool call can therefore be syntactically valid while still violating the domain policy.

We introduce LedgerAgent, an inference-time method that adds an explicit state representation in the agent loop. It has two deterministic components. First, it maintains a schema-anchored ledger which projects successful tool returns into a compact typed dictionary keyed by canonical paths. This requires no additional LLM calls to build. The ledger is re-injected at each turn, so the agent can consult current task state by lookup rather than searching raw transcript history. Second, LedgerAgent applies a policy gate before environment-changing tool calls are executed. The gate evaluates proposed calls against domain rules expressed as predicates over ledger fields. If a proposed call violates policy, the gate blocks it and returns feedback identifying the violated rule and conflicting state.

Contributions:
1. Identify state grounding as a key failure mode in policy-adherent tool-calling agents: agents may retrieve the right records but later act on stale, missing, or incorrectly reconstructed state.
2. Introduce LedgerAgent, an inference-time method that maintains observed state in a schema-anchored typed ledger, renders it for generation, and uses it to check environment-changing calls before execution.
3. Show that LedgerAgent improves consistency-oriented pass^k across customer-service domains and backbone models, with the largest gains on tasks requiring environment-changing actions.

3 Method

LedgerAgent adds two deterministic components: a ledger that stores observed state from successful tool returns, and a policy gate that checks environment-changing calls before execution.

3.1 Ledger State and Updates

Task state is the snapshot of task-relevant facts, conditions, identifiers, and data observed through interaction with the environment. The ledger stores the portion observed through tools in a domain schema. Formally, the ledger is a typed dictionary L: P -> V, where P is the set of canonical schema paths and V is the set of tool-returned values. Paths are stable addresses for observed records, such as user, orders.*, products.*, reservations.*. Nested values remain inside stored records.

The ledger updates only from successful read-tool returns. Failed tools and write-tool returns do not update states. After a successful write, the agent must issue a read call to observe the new state. This observe-not-assume rule keeps the ledger grounded in the external system.

3.2 Ledger-Grounded Generation

Before each model call, LedgerAgent adds the full ledger block to the prompt. Each entry is shown under a canonical path, such as orders.1234 or products.5678, together with the stored returned value. The purpose is to make the current observed state easy for the model to find.

3.3 Policy Gate

The policy gate runs immediately before any environment-changing call is executed. It evaluates the proposed call against executable predicates over the current ledger L and returns one of three outcomes:
- allow: execute the call unchanged.
- revise: remove the call and give the model the violated predicate.
- block: block the call and refuse the requested action.

Predicates are specified once per domain as code over ledger fields. In the reported experiments, the policy layer contains 28 deterministic gate predicates: 10 for airline, 12 for retail, 6 for telecom, and none for telehealth. They encode recurring checks such as ownership, entity-state preconditions, argument grounding, refund or payment consistency, and loop prevention.

3.4 Agent Loop

On each turn: new read-tool returns are absorbed into the ledger; the ledger is rendered into the prompt; the model generates a response or tool call; before any environment-changing call reaches the environment, the policy gate checks it against ledger state.

4 Experiments

Evaluated across four customer-service domains from τ2-bench and τ-Trait, using a mixed panel of open- and closed-weight models. LedgerAgent improves pass^k on majority of the domain-model pair evaluated. The gains are largest at higher values of k, where the standard prompt-based approach is least consistent across independent trials. Ablations show that the improvement comes from the typed ledger and policy-gated action.

5 Conclusion

LedgerAgent addresses a fundamental representation problem in tool-calling agents: task state is implicit in the prompt rather than represented explicitly. By maintaining a schema-anchored ledger and a pre-action policy gate, LedgerAgent changes the failure boundary from post-hoc correction to prevention. Future work includes supporting richer predicate logic, automatic predicate generation from policy documents, and application to non-customer-service domains.
