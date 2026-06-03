---
source_url: https://arxiv.org/html/2602.13258v1
ingested: 2026-05-18
sha256: bebd73dc0e0a0dbc3b3f2f89006eee7555e0c928010d69654d7f56632f8aecec
---
# MAPLE: A Sub-Agent Architecture for Memory, Learning, and Personalization in Agentic AI Systems

**Source:** arXiv:2602.13258v1 | Conference: ALA 2026 at AAMAS 2026

## Overview

MAPLE (Memory-Adaptive Personalized LEarning) proposes a **principled architectural decomposition** of three capabilities that current AI systems incorrectly treat as unified: Memory, Learning, and Personalization. Each requires different infrastructure, operates on different timescales, and benefits from independent optimization.

> **Key Insight:** "Memory stores what we know. Learning extracts what we should know. Personalization applies what we've learned."

## The Core Problem

Current AI assistants fail because they conflate three distinct mechanisms:

| Capability | Question Answered | Failure Mode Without It |
|------------|-------------------|------------------------|
| **Memory** | "What do we know about this user?" | Cannot adapt to known preferences |
| **Learning** | "What should we know based on what happened?" | Repeats errors forever |
| **Personalization** | "Given what we know, how should we respond differently?" | Optimizes for average user (doesn't exist) |

### Running Example: Sarah vs. Marcus

Two users ask: *"What is a transformer in AI?"*

- **Sarah** (Senior ML Engineer): Stored negative feedback on oversimplified responses. Gets: *"Here's the PyTorch implementation of multi-head attention..."*
- **Marcus** (Product Manager, 3 weeks in AI): Consistently engaged with analogies. Gets: *"Think of a transformer like a team of readers working on a document together..."*

## MAPLE Architecture

### Core Principle
```
Memory (ℳ) → retrieves → Personalization (𝒫) → adapts → response → feedback → Learning (ℒ) → writes → Memory
```

**Key architectural separation:**
- **Request path** (milliseconds): Memory + Personalization
- **Background** (minutes to hours): Learning

### System Architecture

Three independent sub-agents, each with specialized tooling and dedicated LLM instances:

| Component | Responsibility | Tools | Answers |
|-----------|---------------|-------|---------|
| **Memory (M)** | Storage and retrieval infrastructure | DB connectors, vector stores, KG interfaces | "What do we know?" |
| **Learning (L)** | Intelligence extraction engine | Analytics engines, pattern recognition | "What should we know?" |
| **Personalization (P)** | Real-time adaptation layer | Context managers, user model interfaces | "How should we respond differently?" |

## Memory (ℳ): Storage Infrastructure

### Three Lenses for Classification

**Lens 1: By Form**

| Type | Description | Trade-off |
|------|-------------|-----------|
| **Token-level (ℳₜ)** | Explicit discrete units (text, records, graph nodes) | ✅ Transparent, editable, debuggable |
| **Parametric (ℳθ)** | Model weights/LoRA adapters | ✅ Seamless; ❌ Opaque |
| **Latent (ℳ_z)** | Hidden states, KV caches | ✅ Efficient; ❌ Ephemeral |

**Lens 2: By Storage Structure**

- **Flat (1D):** Unordered collections (vector databases)
- **Planar (2D):** Explicit relationships (knowledge graphs)
- **Hierarchical (3D):** Multi-layer abstraction (raw → summaries → user models → cohort patterns)

**Lens 3: Cognitive Analogy**

- **Working memory:** Current session context
- **Episodic memory:** Specific events with temporal anchoring
- **Semantic memory:** Abstracted facts, decontextualized knowledge
- **Procedural memory:** Skills and learned behaviors

## Learning (ℒ): Intelligence Extraction

### Symbolic vs. Gradient-Based Learning

MAPLE uses **symbolic learning**—extracting and storing insights in external data structures (not updating model weights):

- ✅ **No catastrophic forgetting** — insights about Sarah don't overwrite Marcus's knowledge
- ✅ **Immediate editability** — users can inspect, correct, or delete learned preferences

### What Learning Extracts

**Facts:** Objective truths (role, team, tenure)
**Preferences:** Interaction patterns (code examples vs. analogies)
**Experiences:** Specific successful interactions worth preserving

### Levels of Learning

| Level | Description | Example |
|-------|-------------|---------|
| **1: Replay** | Store successful trajectories, retrieve by similarity | Sarah's transformer question worked; imitate for attention |
| **2: Strategy Extraction** | Identify patterns across cases | "Users asking about architecture while debugging prefer theory-to-error connections" |
| **3: Skill Synthesis** | Create new capabilities from accumulated experience | Synthesize utility functions when repeatedly generating similar code |

## Personalization (𝒫): Real-Time Adaptation

### Two Modes

1. **Direct Bias:** Modify prompts/context based on user model
2. **Indirect Bias:** Post-hoc response ranking/reordering

### User Model Contents

- Explicit preferences (stated by user)
- Implicit preferences (inferred from behavior)
- Contextual factors (time of day, device, task type)

## Key Differences from Mem0

| Aspect | Mem0 | MAPLE |
|--------|------|-------|
| **Scope** | Memory-only | Memory + Learning + Personalization |
| **Learning** | Implicit in retrieval | Explicit symbolic extraction |
| **Personalization** | Not a first-class concern | Core architectural primitive |
| **Sub-agents** | Monolithic | Three independent specialized agents |

## Source

https://arxiv.org/html/2602.13258v1