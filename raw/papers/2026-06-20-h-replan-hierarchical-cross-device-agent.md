---
source_url: https://arxiv.org/abs/2606.20487
ingested: 2026-06-20
sha256: e345def88ad510fde5c44d7b164c401f168f60ee8033cd1abcf4b2e9e5f8e6b0
---

Beyond Global Replanning: Hierarchical Recovery for Cross-Device Agent Systems
Shu Yao, Yuhua Luo, Qian Long, Jingru Fan, Zhuoyuan Yu, Yuheng Wang, Lin Wu, Yufan Dang, Huatao Li, Chen Qian
1 School of Artificial Intelligence, Shanghai Jiao Tong University
2 Shanghai Innovation Institute  3 Southeast University  4 Tsinghua University

Abstract

Real-world computer-use tasks often span multiple applications and devices, requiring agents to coordinate heterogeneous environments under dynamic runtime failures. Existing multi-device agent systems support task decomposition and cross-device assignment, but recovery remains largely coarse-grained: when execution fails, they typically retry the same strategy, reassign the subtask, or revise the global plan, without systematically modeling the device-local strategy space. This limits their ability to distinguish failures that can be repaired within the current device from those that require cross-device replanning. We propose H-RePlan, a hierarchical replanning framework for multi-device agents with unified API–CLI–GUI execution. H-RePlan equips each device with interchangeable execution strategies and separates device-local strategy recovery from orchestrator-level global replanning through a compact cross-layer failure abstraction. To evaluate this capability, we introduce HeraBench, a fault-injected benchmark that constructs cross-device workflows over Linux and Android devices and injects strategy- and device-level failures. Experiments show that H-RePlan substantially outperforms single-strategy and coarse-grained multi-device baselines, achieving higher completion, instruction adherence, and perfect-pass rates while reducing the token cost required for reliable end-to-end success. These results demonstrate that scope-aware hierarchical recovery is essential for robust multi-device agent execution.

1 Introduction

Large language model agents have shown strong capabilities in assisting users with computer-based tasks, including web navigation, desktop GUI operation, API use, code and command execution, and cross-application automation. In practice, many real-world tasks span multiple applications and devices. For example, submitting an expense claim may require collecting invoices from mobile apps, transferring them to a laptop, organizing local files, and uploading them to a reimbursement system. Such tasks require agents to coordinate across devices with different files, credentials, and application states.

Recent multi-device agent systems have enabled agents to decompose tasks, assign subtasks to devices, and execute them sequentially or in parallel. Meanwhile, single-device agents have shown that API, CLI, and GUI strategies offer complementary strengths: APIs provide structured access, CLIs support scriptable system-level operations, and GUIs remain broadly available through human-facing interfaces. However, existing multi-device systems typically expose each device agent through only one primary execution strategy, such as GUI-only or CLI-only. This limits runtime recovery: errors are either escalated to cross-device reassignment or left unresolved, even when local recovery would suffice.

To address this challenge, we propose H-RePlan, a hierarchical replanning framework for multi-device agents with unified API–CLI–GUI execution. H-RePlan introduces a platform-independent strategy-control abstraction that represents each device by its supported subset of API, CLI, and GUI execution strategies. At the device layer, a Strategy Planner decomposes assigned subtasks, selects execution strategies, and performs local recovery by revising the strategy or instruction when a strategy-level failure occurs. At the system layer, an Orchestrator maintains the cross-device task plan, incorporates failure evidence from devices, and handles device-level failures through reassignment or recovery subtasks.

To evaluate hierarchical recovery, we design HeraBench, a fault-injected benchmark for multi-device workflows. HeraBench constructs tasks over Linux and Android devices and injects strategy- and device-level failures that require both local strategy recovery and cross-device replanning.

Contributions:
• H-RePlan: scope-aware hierarchical replanning framework separating device-local strategy recovery from orchestrator-level cross-device replanning
• Platform-independent unified strategy-control abstraction equipping each device with interchangeable API, CLI, GUI strategies
• HeraBench: fault-injected multi-device benchmark evaluating hierarchical recovery under strategy- and device-level failures

2 Related Work

Execution strategies and replanning: LLM agents interact through tools, APIs, command lines, code execution, and GUIs. Prior work studies feedback-driven action revision, failed-trial reflection, plan refinement, failure-aware decomposition, GUI-agent replanning, and test-time planning allocation. H-RePlan builds on this replanning perspective and extends it to multi-device settings.

Multi-device agents: CRAB constructs tasks over desktop and mobile environments with a ReAct-style interaction loop. UFO3 decomposes user requests into mutable task constellations, assigns subtasks to devices, propagates information, and updates plans in response to execution events. Despite this progress, existing multi-device replanning remains coarse-grained: recovery is mainly organized around retries, task updates, or device-level reassignment, while device-local strategy revision is not systematically modeled.

3 Methodology

3.1 Problem Formulation
Cross-device collaboration fulfills a natural-language instruction I through a sequence of device-grounded steps S = ⟨s1, s2, …, sm⟩, where each step pairs a semantic intent with an expected target device. The system operates over heterogeneous devices D = {d1, d2, …, dn}. Each device d possesses a dynamic capability profile containing OS, applications, capabilities, and available execution strategies Πd.

Because runtime conditions fluctuate (e.g., network drops or token expiration), static planning is insufficient. The system maintains a dynamic plan P composed of sequential subtasks based on the active execution context. Under failures, the system triggers replanning to yield an updated plan.

3.2 Hierarchical Replanning Overview
H-RePlan organizes cross-device execution as a closed-loop process among a global plan, strategy execution agents, and the external environment. The Orchestrator first converts the instruction and device profiles into an ordered cross-device plan, then dispatches each subtask to its assigned device. On the device side, the Strategy Planner decomposes the subtask, chooses an appropriate execution strategy, and sends a strategy-specific instruction to the corresponding API, CLI, or GUI agent.

This hierarchy separates system-level recovery from device-level recovery: the Strategy Planner handles failures that can be resolved within the current device by changing or continuing local strategies, while the Orchestrator intervenes only when device-level feedback indicates that remaining work requires cross-device reassignment, downstream context revision, or global plan repair.

3.3 Orchestrator
The Orchestrator operates in three modes:
• Task Creation: Given the user instruction and available device profiles, decomposes the request into a sequential subtask chain
• Information Append: After a subtask succeeds, appends relevant context to downstream dependent subtasks
• Global Replanning: When a subtask fails, receives structured failure evidence and rewrites the remaining chain through retry with modified instruction, rerouting to another device, inserting recovery subtasks, preserving reusable partial progress, or declaring global failure

3.4 Strategy Planner
The Strategy Planner is the device-level planner responsible for selecting and revising execution strategies within a single device. It maps the assigned subtask and previous observations to one of three decisions: Execute(π, x), Done(y), or Escalate(c).

The Strategy Planner operates in three states:
• Create: analyzes assigned subtask and forms device-local execution plan, emits first Execute decision
• Progress Check: examines result of previous successful execution step
• Replan: reacts to failed local execution step, preserves completed local progress, selects another available strategy

Available strategies: API (structured functions), CLI (local shell and file operations), GUI (user-interface interaction when structured access is insufficient).

3.5 Cross-Layer Failure Event (CLFE)
A key question is how much failure information should cross the boundary from device-level execution to system-level planning. A binary failure signal is too weak; passing full low-level traces overloads the global planning context.

H-RePlan introduces the Cross-Layer Failure Event (CLFE) as a compact, planning-oriented failure abstraction. CLFE encapsulates five key elements: identity of failed subtask, source device, categorized failure type, summary of local strategy attempts with observations, and explicit reasoning for why escalation is necessary.

CLFE bridges local execution and global replanning by exposing recovery-relevant facts. It tells the Orchestrator what was attempted, what was observed, which partial outputs remain reusable, and why device-local recovery stopped.

3.6 Strategy Execution Agents
H-RePlan implements three complementary agents: API Agent (reliable structured service access), CLI Agent (local computation and file-system manipulation), GUI Agent (user-interface interaction when structured access is insufficient).

4 Experiment

4.1 HeraBench
HeraBench comprises 23 seed tasks expanded into 174 evaluation variants. Each episode executes across a four-device environment (two Linux + two Android devices), operating real services and local files. Variants are deterministically injected with local faults (disabling specific strategies while leaving same-device alternatives open), global faults (removing all same-device recovery paths), or mixed faults.

Metrics: Task Completion (% of required postconditions satisfied), Instruction Adherence (ratio of gold steps where agent fulfills required semantic intent on allowed target device), Perfect Pass (100% completion + 100% adherence), Tok./Ep. (average token usage per episode), Tok./PP (expected token cost per perfect pass = Tok./Ep. / perfect-pass rate).

Baselines: CRAB (GUI and API variants), UFO3 (GUI and API variants). DeepSeek-V4-Pro for text, Kimi K2.5 for GUI.

4.2 Main Results
H-RePlan achieves 75.84% completion, 77.72% instruction adherence, and 36.78% perfect-pass rate — substantially outperforming UFO3-GUI's 13.79% perfect-pass rate. H-RePlan reduces expected cost per perfect pass from 10.51M tokens (UFO3-GUI) to 1.93M tokens — a 5.44× improvement. UFO3-API had lowest per-episode token cost but zero perfect-pass rate due to inability to execute local file-system tasks.

Ablation results: removing Global Replan causes strongest degradation; removing Strategy Planner leads to different failure mode (lower adherence); removing CLFE shows binary signal can still trigger some recovery but is consistently worse; removing API Strategy confirms API strategies provide efficient access when available.

Key insight: local faults resolved without early escalation achieve 76.81% completion and 82.00% adherence, vs. 68.89%/62.22% when escalated early — confirming strategy-level failures are best handled through local revision.

5 Conclusion
H-RePlan establishes that robust multi-device agent systems must explicitly model both intra-device strategy spaces and inter-device orchestration. Scope-aware hierarchical recovery enables agents to recover many strategy-level failures locally while reducing unnecessary loss of execution context.