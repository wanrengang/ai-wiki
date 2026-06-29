---
source_url: https://arxiv.org/abs/2606.18448
ingested: 2026-06-19
sha256: 243a4cdc798c2029549e4d967a9703c8c014f5e9668db19b1a645208c931e7de
---
VisualSkill: Multimodal Skills for Computer-Use Agents
Ziyan Jiang, Li An, Yujian Liu, Jiabao Ji, Qiucheng Wu, Jacob Andreas, Yang Zhang, Shiyu Chang
UC Santa Barbara, MIT CSAIL, MIT-IBM Watson AI Lab
arXiv: 2606.18448

Abstract

Computer-use agents (CUAs) approach human-level performance on standardised benchmarks but still struggle on long-horizon tasks and unseen software. Existing skill libraries address this with reusable skills, but represent the skill artifact as text only, despite the visual nature of GUI interaction. We propose VisualSkill: a hierarchical multimodal skill, tailored to each target application and organised as a central index over per-topic files, which the agent consumes through a load_topic MCP tool that fetches the relevant topic's text and figures on demand. We construct each skill with a two-stage pipeline that combines authored documentation with live-application UI exploration. On two CUA benchmarks, CUA-World and OSExpert-Eval, a Claude Code CLI agent backed by Claude Opus 4.6 reaches an average score of 0.456 with VisualSkill, a +15.3 point absolute lift over the no-skill baseline (0.303). Against a matched text-only skill that is generated from the same source content and differs from VisualSkill only in modality, VisualSkill yields a further +8.3 point absolute gain (0.373 vs. 0.456).

Key Results

- No-skill baseline: 0.303 (Claude Opus 4.6 Claude Code CLI)
- Stage 1 VisualSkill (doc-derived): 0.363 (+6.0% over baseline)
- Stage 2 VisualSkill (+ UI exploration): 0.456 (+15.3% over baseline, +9.3% over Stage 1)
- Matched text-only control (Stage 2): 0.373
- Multimodal gain over text-only: +8.3% absolute (0.373 → 0.456)

Per-domain results (Stage 2 VisualSkill):
- OSExpert-Eval: LibreOffice 0.500, GIMP 0.333, Tableau 0.300
- CUA-World: Writer 0.276, Calc 0.656, Impress 0.580, QGIS 0.726, OpenToonz 0.274

Two failure modes where figures help:
1. Identifying UI elements: toolbar icons, palette swatches, graphical controls whose meaning lives in appearance; targets appearing after earlier interactions (modal sub-dialogs, expanded dropdowns, context menus)
2. Verifying UI state between steps: multi-step workflows requiring post-action state verification; silent failures where form fields appear accepted but reject on commit

Method

Skill Definition:
- One skill per target application
- skill.md index: lists each topic with a one-sentence "when to use" description
- Per-topic guides gt = (pt, Ft): text body paired with UI figures
- Agent reads only the index up front; invokes load_topic(t) MCP tool to fetch per-topic guide on demand

Two-Stage Construction Pipeline:

Stage 1 — Authored Documentation:
- Parse table of contents from official user guide (PDF/HTML) as topic set T
- For each topic: extract pages covering t, pull out every figure as Ft, joint-generate pt_mm and pt_txt in one LLM call
- Figures come from official vendor manual (not agent-captured)

Stage 2 — Live UI Exploration:
(a) Free Explorer: Opus-class planner partitions idle app screenshot into K=8 regions; Sonnet-class worker agent spawned per region in isolated Docker, interacts with region and captures cropped screenshots + structured notes
(b) Trajectory-Targeted Explorer: Reviewer agent reads failed training-task trajectories, identifies UI regions the Stage 1 skill underexplains, dispatches second worker pool against those regions
(c) Assembler merges captured screenshots + notes into per-topic skill files

MCP Tool vs. Plain Read:
- Direct Read: agent loads ~10× fewer figures, stops consulting skill within first ~2% of rollout
- MCP load_topic tool: 100% load rate, 7.9 figures/task, skill consulted throughout trajectory
- MCP interface delivers each figure inline with surrounding text in single tool result; keeps skill accessible throughout trajectory

When figures don't help:
Short, explicitly specified action sequences where text alone suffices.

Code: https://github.com/XMHZZ2018/VisualSkills
