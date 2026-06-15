---
source_url: https://arxiv.org/abs/2606.09774
ingested: 2026-06-09
sha256: f613dbf56af492ee7720ecf13b000e85e970794ccf5dd485877943a6d7f51962
---

SIGA: Self-Evolving Coding-Agent Adapters for Scientific Simulation
Matthew Ho, Brian Liu, Jixuan Chen, Audrey Wang, Lianhui Qin
University of California, San Diego

Abstract

Advanced scientific simulators expose specialized input languages that turn simulation goals into executable configurations, but learning and using these interfaces can cost domain scientists hours to days. We study simulator setup as a problem of agent-tool interface grounding: what minimal simulator-specific adaptations are needed for an off-the-shelf coding agent to operate real scientific software? Our intuition is that coding agents already know how to navigate files, edit code, run commands, and repair outputs, but they lack the simulator's executable contract: its vocabulary, structural constraints, validation rules, and termination conditions. We introduce SIGA, a Simulator-Interface Grounding Adapter that supplies this contract through retrieval, procedural memory, in-trajectory validation, and validation-enforced termination. We primarily evaluate SIGA on GEOS, an open-source multiphysics simulator used in subsurface science. SIGA produces a complete GEOS deck in about five minutes with TreeSim above 0.90, matching the quality of an extended-budget human expert who required about three hours, a roughly 36× wall-clock speedup. On a harder held-out set, grounding raises TreeSim from 0.720 to 0.789, a roughly 10% relative gain over the bare agent, and can reduce the across-seed standard deviation by 16×. Self-evolution further improves SIGA by rewriting adapter contents from prior trajectories, yielding the highest held-out GEOS mean and matching or outperforming the strongest hand-designed configuration. Transfers to OpenFOAM and LAMMPS show that the dominant mechanism shifts by interface: validation matters most when structural completeness is the bottleneck, while memory and retrieval matter most when domain correctness is the bottleneck. Together, these results suggest that lightweight, self-improvable grounding layers can turn general coding agents into practical operators of scientific software.

1 Introduction

Scientific simulation is one of the central computational workflows in modern science. Researchers use simulators to study physical, chemical, biological, and engineering processes that would be expensive, slow, dangerous, or impossible to observe directly in the laboratory. In geophysics, simulators such as GEOS (GEOS Development Team, 2024) model CO2 sequestration, reservoir flow, hydraulic fracture, wellbore behavior, and induced seismicity; in molecular science, molecular-dynamics engines such as LAMMPS (Holbrook et al., 2026) encode atomistic systems, force fields, and time-integration protocols. Across these domains, the scientific goal may be expressed in natural language, but the executable simulation must be written in a simulator-specific input language.

This translation from scientific intent to runnable simulator configuration is a persistent bottleneck in simulation-driven research. Advanced simulators expose large domain-specific language (DSL) whose tokens name solver classes, material models, mesh regions, boundary conditions, event schedules, numerical schemes, and output requests. A valid input deck must satisfy syntax constraints, schema constraints, and physical or domain-conventional constraints. Scientists new to a simulator can spend hours or days reading documentation, searching examples, testing numerical settings, and debugging invalid configurations before obtaining a runnable deck.

This setup stage is a natural target for agentic AI because it is both valuable and constrained. It is valuable because reducing setup time directly accelerates scientific iteration. It is constrained because the agent is not being asked to invent a new hypothesis or interpret final physical results; it is being asked to operate an existing scientific tool according to a given specification.

We introduce the Simulator-Interface Grounding Adapter (SIGA), a thin wrapper around an off-the-shelf coding agent. SIGA factorizes grounding into four reusable components: (1) Retrieval gives semantic access to simulator documentation, schemas, examples; (2) Procedural memory keeps high-frequency simulator vocabulary always visible; (3) Agent-callable validator lets the agent check and repair outputs mid-trajectory; (4) Stop-hook makes validation a mandatory termination condition. This decomposition is simulator-agnostic: the same slots instantiate as XML schema checks for GEOS, required-file and dictionary checks for OpenFOAM, and checks over commands, references, force-field declarations, and run-control blocks for LAMMPS.

Four findings: (1) Grounding turns hours of expert setup into minutes at expert-level quality — a ∼36× gain. (2) Grounding raises reliability: 10% relative gain on held-out set, 16× reduction in across-run standard deviation. (3) Self-evolved variant matches or outperforms the best hand-designed configuration. (4) Useful mechanism shifts with simulator interface: validation enforces structural completeness in GEOS and OpenFOAM; memory and retrieval supply domain knowledge in LAMMPS.

2 Related Work

LLM agents for scientific code and simulators include ChemCrow, Coscientist for chemistry; CellVoyager for single-cell analysis; The AI Scientist for end-to-end ML research; OpenFOAM-oriented CFD agents; molecular-dynamics agents; finite-element agents; reservoir-simulation agents. The shared design lesson: general-purpose LLMs are insufficient for high-fidelity simulator use unless wrapped with domain grounding, structured interface abstractions.

3 Background: GEOS

GEOS is an open-source multiphysics simulator used in subsurface science (CO2 sequestration, reservoir flow, hydraulic fracture). It uses a custom XML-based input deck language. Scientists need hours to days to produce a valid GEOS deck.

4 Method: SIGA

SIGA wraps an off-the-shelf coding agent (e.g., Claude Code, OpenHands) with four grounding components:
- Retrieval: semantic access to documentation and examples
- Procedural Memory: always-visible high-frequency vocabulary and patterns
- In-trajectory Validator: agent-callable validation mid-trajectory
- Validation-Enforced Termination: mandatory stop condition based on validation

Self-evolution: SIGA rewrites its own grounding components (primer, memory, auxiliary skills) from prior successful trajectories, without hand-tuning.

5 Experiments

Tasks: GEOS deck generation from natural language specification.
- Representative task: geoscience expert new to GEOS took ~3 hours; SIGA-equippd agent matched expert quality in ~5 minutes (36× speedup).
- Hard held-out set: grounding raises TreeSim from 0.720 to 0.789 (+10% relative); reduces across-seed std dev by 16×.
Transfer to OpenFOAM and LAMMPS: dominant mechanism shifts by interface — validation matters most for structural completeness, memory/retrieval matter most for domain correctness.

6 Results

- SIGA self-evolution matches or outperforms best hand-designed configuration
- Lightweight adapter turns general coding agents into practical scientific software operators
- Minimal grounding design gives concrete recipe for adapting agents to new simulator interfaces