---
source_url: https://arxiv.org/abs/2606.12797
ingested: 2026-06-14
sha256: 996e2be7b950f7cc3f2a1eb88b462f6fe6b64e0750832835540a90982d308e3e
---

The Containment Gap: How Deployed Agentic AI Frameworks Fail Public-Facing Safety Requirements
Md Jafrin Hossain, Mohammad Arif Hossain, Weiqi Liu, Nirwan Ansari

## Abstract

Agentic large language model systems that autonomously invoke tools, maintain persistent memory, and execute multi-step plans are increasingly deployed in public-facing domains, including government services, healthcare triage, and financial advising. We ask whether the frameworks used to build these systems provide architectural-level structural safety guarantees. Applying six containment principles derived from a compositional model of agentic architectures, we audit three dominant frameworks (LangChain, AutoGPT, and OpenAI Agents SDK) and find no native compliance in any of them. Memory integrity, a defense against one of the most prevalent vulnerability classes, is not observed in any of the three evaluated frameworks. We validate these findings empirically: in a simulated government benefits agent built on LangChain, a single memory-poisoning write induces persistent targeted corruption across all tested seeds and backends, increasing the wrongful denial rate for targeted applicants to 88.9%. Under a complex five-factor policy, the same attack preserves aggregate accuracy while increasing targeted wrongful denials by 3.5×, rendering the corruption difficult to detect through standard monitoring. We then introduce two lightweight containment mechanisms: a memory integrity validator and a policy gate, which eliminate both attack vectors with sub-millisecond overhead (<0.2ms per call). We conclude that the current agentic framework ecosystem may not yet meet secure-by-default expectations for public-facing deployments and outline priority architectural interventions to enable trustworthy deployment in high-stakes, socially impactful applications.

## 1 Introduction

Agentic AI systems are increasingly deployed in public-facing domains such as government services, healthcare, and finance. Unlike traditional LLM chatbots, these systems invoke tools, maintain persistent memory, and act autonomously over multi-step horizons. A single corrupted reasoning cycle can propagate through tool execution into memory, poisoning subsequent interactions and potentially leading to persistent, system-level failures with real-world consequences.

This paper makes four contributions:
1. First audit methodology that operationalizes formal containment principles into a reusable compliance matrix for agentic frameworks.
2. Auditing LangChain, AutoGPT, and OpenAI Agents SDK suggests no native compliance across any of the six principles.
3. A single memory poisoning write induces targeted corruption across five backends, and under a five-factor policy, can remain difficult to detect through aggregate metrics.
4. Two deterministic interventions substantially reduce attack success rates with sub-millisecond overhead.

## 2 Background: The Composition Problem

### 2.1 Agentic Systems as Compositional Pipelines

An agentic LLM system composes four functional stages in a recursive loop: perception (P), reasoning/behavior (B), execution (E), and memory update (U). The decision-to-action mapping at each timestep is: Φ(ot, mt) = E(B(P(ot), mt)).

Individual stages may be secure in isolation, but their composition becomes vulnerable without inter-layer isolation. Corrupted outputs propagate across stages.

### 2.2 Execution Containment

Security requires that Φ(ot, mt) ∈ C for all t, where C ⊆ A is a policy-constrained safe action space. When E directly forwards B's output to the runtime without such projection, the system operates in a state of autonomy without containment.

### 2.3 Six Containment Principles

1. **P1: Reasoning-Execution Separation** — Policy gates π lie between planning and execution so that the agent cannot implement every plan it devises.
2. **P2: Capability Scoping** — A bounded token Tk is given to each session defining which tools can be used, parameter ranges, rate limits, and expiry times.
3. **P3: Memory Integrity** — The validity of any write before it reaches long-term memory is tested by an integrity function I. Those writings that do not pass the test are discarded.
4. **P4: Layer-Transition Validation** — Security checks are performed at all interfaces (P→B, B→E, E→U).
5. **P5: Authenticated Communication** — All messages between agents should contain credentials such as digital signatures.
6. **P6: Runtime Monitoring** — An anomaly detector monitors execution paths, activating containment if an anomaly is detected.

### Theorem 1 (Containment Sufficiency)

If an agentic system satisfies P1 (policy-gated execution) and P3 (validated memory writes), then no single-step memory-poisoning attack can induce a persistent policy violation.

## 3 Audit Methodology

Framework selection: LangChain Agents, AutoGPT, OpenAI Agents SDK.
Evidence sources: official documents, source code examination, security studies.
Scoring rubric: ✓ = native default (enabled without configuration); ✓* = requires explicit configuration; ✗ = absent.

## 4 Results: The Compliance Matrix

| Principle | LangChain | AutoGPT | OpenAI SDK |
|-----------|-----------|---------|------------|
| P1: Reasoning-Exec Sep | ✓* (Callbacks) | ✗ | ✓* (Guardrails) |
| P2: Capability Scoping | ✓* | ✗ | ✓* |
| P3: Memory Integrity | ✗ | ✗ | ✗ |
| P4: Layer-Trans Validation | ✓* | ✗ | ✓* |
| P5: Auth Communication | ✗ | ✗ | ✓* |
| P6: Runtime Monitoring | ✓* | ✓* | ✓* |

**Four systemic patterns:**
1. Zero native compliance — no framework achieves ✓ on any principle
2. Universal memory integrity failure — P3 scores ✗ across all three
3. Safety is optional — safeguards require explicit configuration, violating secure-by-default
4. Autonomy inversely correlates with compliance — AutoGPT (most autonomous) has 5 non-compliant principles

## 5 Experimental Validation

### Setup
Scenario: LangChain-based welfare benefits agent, 250 synthetic claims, Qwen-2.5 3B-Instruct via Ollama.

### Experiment 1: Memory Poisoning
Single adversarial memory write (fake policy: Region B income<$30k should be denied) injected at claim 11 of 40.
- Corruption rate: **1.000** across all three seeds
- Mean accuracy collapses from 0.908 to 0.558 (35 percentage point drop)
- Region B wrongful denial rate: **88.9%**

### Experiment 2: Memory Validator Recovery
With memory integrity validator active (deterministic regex checking source provenance, schema conformance, demographic-targeting patterns):
- Corruption rate drops from 1.000 to **0.000**
- Accuracy recovers to 0.967
- Overhead: **0.016ms per call**

### Experiment 3: Tool Policy Bypass
Without gate: 100% bypass rate (path traversal, unauthorized API calls, restricted writes).
With policy gate (deny-all allowlist + path canonicalization): **0.000 bypass rate**, 0.129ms overhead.

### Cross-Model Generalization
Memory poisoning achieves **1.000 corruption rate** on Claude Haiku 4.5 AND GPT-4o — even RLHF-aligned commercial models are equally vulnerable because alignment happens after the fact while attacks target upstream memory.

### Complex Policy Concealment
Five-factor policy (income, household size, dependents, prior benefits, residency):
- Overall accuracy: nearly unchanged under poisoning (conceals harm)
- Region B wrongful denial: **3–3.5× increase**
- Validator restores baseline, reducing corruption from 1.000 to 0.000 (Claude) / 0.167 (GPT-4o-mini)

## 6 Discussion and Recommendations

**Equity implications:** On a system processing 50,000 applications/month, ~8,900 erroneous denials/month for targeted group.

**Three prioritized interventions:**
1. **Tool declaration as capability scope (P2)** — deny-all by default, allowlist per session
2. **Policy gates between reasoning and execution (P1)** — reject out-of-scope actions by default
3. **Provenance-verified memory writes (P3)** — validate and reject untrusted inputs by default

## 7 Related Work

Existing surveys extensively map the agentic AI attack landscape but treat vulnerability types as independent phenomena without a structural propagation model. This paper bridges formal containment analysis, empirical validation, and deployment readiness in public-facing systems.

## 8 Conclusion

No native structural containment guarantees in evaluated frameworks. A single memory poisoning write can deny benefits to 88.9% of eligible applicants in an affected area. Two lightweight deterministic containment mechanisms substantially reduce attack success rates with <1ms overhead. Secure-by-default is not yet prioritized in current framework design. Populations who rely on publicly deployed AI systems are unable to assess the underlying technology's safety.

Code: https://anonymous.4open.science/r/containment-ai-agent-sec-13B6