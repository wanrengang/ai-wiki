---
source_url: https://arxiv.org/html/2606.05080
ingested: 2026-06-05
sha256: e762ca35181c683690e044c1b813cc3d0deb2cbee749a86dcb290184a014ca53
---

AutoLab: Can Frontier Models Solve Long-Horizon Auto Research and Engineering Tasks?
Zhangchen Xu, Junda Chen, Yue Huang, Dongfu Jiang, Jiefeng Chen, Hang Hua, Zijian Wu, Zheyuan Liu, Zexue He, Lichi Li, Shizhe Diao, Jiaxin Pei, Jinsung Yoon, Hao Zhang, Mengdi Wang, Radha Poovendran, Misha Sra, Alex Pentland, Zichen Chen

Abstract

Scientific and engineering progress is fundamentally a long-horizon iterative process: proposing changes, running experiments, measuring outcomes, and continuously refining artifacts. Yet existing benchmarks for frontier models primarily evaluate either single-turn responses or short-horizon agent trajectories, failing to capture the challenges of sustained iterative improvement over extended time horizons. To address this gap, we introduce AutoLab, a new benchmark for ultra long-horizon closed-loop optimization. AutoLab consists of 36 realistic, expert-curated tasks spanning four diverse domains: system optimization, puzzle & challenge, model development, and CUDA kernel optimization. Each task begins with a correct but deliberately suboptimal baseline and challenges agents to improve it within a strict wall-clock budget. Evaluating 17 state-of-the-art models reveals the dominant predictor of success is not the quality of an agent's initial attempt, but its persistence in repeatedly benchmarking, editing, and incorporating empirical feedback. While claude-opus-4.6 exhibits strong long-horizon optimization capabilities, most frontier models, including several proprietary ones, either terminate prematurely or exhaust their budgets with minimal progress. These results underscore the importance of time awareness and persistent iteration in autonomous agents. We open-source the full benchmark, evaluation harness, and task artifacts, to accelerate research toward truly capable long-horizon agents.

Code: autolabhq/autolab | Website: autolab.moe

1 Introduction

Frontier LLM agents are increasingly deployed on tasks that play out over hours rather than minutes, from post-training models (Rank et al., 2026) and optimizing low-level systems (Chi et al., 2026) to running open-ended research loops (Novikov et al., 2025, Karpathy, 2026). Progress on such tasks is iterative: it comes from inspecting an artifact, proposing a change, running experiments, measuring the outcome, and refining over many cycles, not from a single correct answer. Sustaining this loop over a long horizon requires managing time, compute, and noisy empirical signals.

AutoLab addresses this gap with 36 tasks across 4 categories: system optimization, puzzle & challenge, model development, CUDA kernel optimization. claude-opus-4.6 leads every sub-domain with Avg@3 of 0.68 vs 0.50 for the next-best model. gpt-5.4 fails for reasons unrelated to raw coding ability: some terminate after minimal exploration, others exhaust budgets without producing valid final solutions. Key finding: final performance correlates more strongly with persistence than with one-shot solution quality.

2 The AutoLab Benchmark

Each task provides: instruction, environment (containerized sandbox), verifier, reference solution, wall-clock budget. The agent receives the instruction and environment inside the sandbox, and must produce a modified implementation that the verifier evaluates within the allotted budget.

Scoring: Anchored relative scoring (log-stretch for performance metrics, linear for bounded quality metrics). s=0 at baseline, s=0.5 at reference, s approaching 1.0 as performance nears practical optimum. Minimum-improvement gate ensures s=0 until agent exceeds baseline.

3 Benchmark Results

Total: 2,544 wall-clock hours and 8.60 billion tokens across 17 models. claude-opus-4.6 leads all four categories. Key behavioral limitations: lack of time awareness (premature termination vs budget exhaustion). Dominant predictor of success is persistence in repeated benchmarking, editing, and incorporating empirical feedback.

4 Analysis

Trajectory analysis of 302 zero-score rollouts reveals that agents that repeatedly benchmark, edit, and incorporate empirical feedback throughout the trajectory achieve substantially better outcomes. The dominant predictor of final performance is not the quality of the agent's initial solution, but its persistence in iterative refinement.

5 Related Work

Related to AlphaEvolve (Novikov et al., 2025), Karpathy's AutoResearch agent (Karpathy, 2026), and other long-horizon optimization benchmarks. AutoLab differs in that it simultaneously offers broad coverage across scientific and engineering domains while maintaining high difficulty and resistance to saturation.

6 Conclusion

Persistence, time awareness, and empirical search will be central to future autonomous research agents. AutoLab open-sources full benchmark, evaluation harness, and task artifacts.
