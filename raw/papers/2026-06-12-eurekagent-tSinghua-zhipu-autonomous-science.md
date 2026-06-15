---
source_url: https://arxiv.org/html/2606.13662
ingested: 2026-06-12
sha256: 88b615f4ad8e30734988740a3fa572ec36f59fb53888ce0054e2278b75bbc810
---

 EurekAgent: Agent Environment Engineering is All You Need for Autonomous Scientific Discovery
Amy Xin1, Jiening Siow1, Junjie Wang1, Zijun Yao1,
Fanjin Zhang2, Jian Song2, Lei Hou1, Juanzi Li1
1Department of Computer Science and Technology, Tsinghua University  2Zhipu AI

Abstract

LLM-based agents have shown increasing potential in automating scientific discovery. Given an optimizable metric and an execution environment, they can propose, validate, and iterate scientific solutions, and have produced results that outperform human-designed approaches. As model capabilities continue to improve, we argue that the bottleneck for autonomous scientific discovery is shifting from prescribing agent workflows to designing agent environments—the resources, constraints, and interfaces that shape agent behavior. We frame this as environment engineering: building environments that amplify productive behaviors, such as open-ended exploration, systematic artifact management, and inter-agent collaboration, while suppressing harmful behaviors, such as reward hacking and high-friction human oversight. We present EurekAgent, an environment-engineered agent system for metric-driven autonomous scientific discovery. EurekAgent engineers the environment along four dimensions: permissions engineering for bounded agent execution and isolated evaluation; artifact engineering for filesystem and Git-based collaboration; budget engineering for budget-aware exploration; and human-in-the-loop engineering for easy human supervision and intervention. EurekAgent sets new state-of-the-art results on multiple mathematics, kernel engineering, and machine learning tasks, including new state-of-the-art 26-circle packing results discovered with less than $11 in total API cost. We open-source our code and results, and call for environment engineering as a core research direction for developing reliable autonomous research agents.

1Introduction

Large language models are increasingly transforming scientific discovery from manual trial-and-error to computational exploration: in domains where research progress can be measured by an optimizable metric, LLM-based agents can autonomously propose hypotheses, run experiments, observe feedback, and iterate solutions, reducing human effort in method tuning while largely expanding the scale of exploration.

Most existing autonomous research systems realize this vision by prescribing research-specific agentic workflows. Evolutionary systems such as AlphaEvolve explicitly maintain populations of candidate programs and use evaluator feedback to guide mutation and selection. Machine learning systems such as AIDE organize exploration around solution trees, feedback loops, and role-specialized agents. While these designs can be effective, they encode strong assumptions about how research should proceed. As general-purpose coding agents like Claude Code and Codex become stronger, recent evidence suggests that much of the useful capability may already reside in the base agent: given a clear research task and an optimizable metric, these agents can already discover new state-of-the-art scientific solutions. On ResearchClawBench, both Claude Code and Codex, used as standalone general-purpose agents, outperform all evaluated research-specific agent systems.

However, task performance alone does not make reliable autonomous researchers. Scientific discovery requires rigor, reproducibility, and inspectability, yet agents may contaminate evaluations, manipulate artifacts, or fail to follow procedural constraints. Such reward-hacking and observability failures have already been reported in agentic research systems. Therefore, trusting agents without environmental constraints can lead to impressive but unreliable results.

These observations suggest that as general-purpose agents become more capable, the bottleneck for autonomous scientific discovery is shifting from prescribing agent behavior through detailed workflows to engineering the environments in which agents operate. We frame this as environment engineering.

We present EurekAgent, an agent system for autonomous scientific discovery that coordinates off-the-shelf CLI agents through four environment engineering dimensions: (1) permissions engineering, (2) artifact engineering, (3) budget engineering, and (4) human-in-the-loop engineering.

3EurekAgent

EurekAgent is an environment-engineered agent system for metric-driven research tasks. Given a problem description, a hidden evaluation script, a submission-format specification document, optional initial code, and time and API cost budgets, EurekAgent coordinates multiple sessions of off-the-shelf CLI agents to autonomously propose and iterate high-scoring solutions. Instead of prescribing a detailed research workflow, EurekAgent engineers an outer environment that organizes agent activity through a simple three-stage loop: Prepare → [Propose_r → {Implement_r,p}]_p=1..P]_r=1..R.

Prepare Stage: Before iteration begins, EurekAgent launches a preparation agent session to set up a reliable runtime setup. The agent reads the problem description, the evaluator-facing submission requirements document, and optional initial code; tests the hidden evaluation service; and installs or validates required runtime dependencies.

Propose Stage: At the beginning of each iteration round, EurekAgent launches a proposal agent session to generate diverse initial hypotheses for the next round of solution optimization.

Implement Stage: The implement stage is the fan-out step. For each proposed hypothesis, EurekAgent launches a separate implementation agent session in parallel and assigns it a separate workspace.

3.2Environment Engineering in EurekAgent

Permissions Engineering: Scientific discovery agents need broad capabilities, but unconstrained capability can compromise research integrity. EurekAgent imposes system-level permission boundaries. Each run executes inside a Docker container with a mounted workspace. The hidden evaluator with possible test data are kept outside the agent-visible workspace and exposed only through a secure grading service: agents can submit candidates and receive official scores, but cannot inspect or modify the evaluator itself. EurekAgent also enforces same-round isolation among parallel implementation sessions.

Artifact Engineering: EurekAgent uses the filesystem coupled with Git history as shared long-term memory. The filesystem stores stage deliverables for cross-session communication. Git commits track solution evolution.

Budget Engineering: EurekAgent controls resources along two axes: wall-clock time and API cost. EurekAgent tracks accumulated token usage across sessions. Budget control also supports operational continuity for long-running research processes.

Human-in-the-loop Engineering: EurekAgent provides two complementary interfaces: a terminal UI and a web monitor.

4Experiments

We evaluate EurekAgent on three domains: mathematics, kernel engineering, and machine learning engineering. All experiments use EurekAgent with Claude Code as the CLI agent and GLM-5.1 as the base LLM.

4.1Mathematics: Circle Packing (26 circles, unit square), Erdős' minimum overlap, first autocorrelation inequality. EurekAgent achieves new state-of-the-art across all three tasks.

4.2Kernel Engineering: Tasks include TriMul (kernel execution time optimization).

4.3MLE-Bench: EurekAgent ranks first on the evaluated MLE-Bench subset with 85.71% (vs. prev. best AI 71.43%).

Key Results: 26-circle packing: EurekAgent 2.635999 vs. Prev. Best AI 2.635986. API cost for circle packing: <$11 total.