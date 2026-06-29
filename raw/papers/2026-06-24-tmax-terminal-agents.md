---
source_url: https://arxiv.org/abs/2606.23321
ingested: 2026-06-24
sha256: 594d0f43e5a05d0f086f7a5ca4e866d14cabeeb0899f35f4537106b4bba0acc3
---

Abstract                                              agent datasets. We then train open-weight
                                                                                                                                           models using RL with our data, using a sim-
                                                                  Terminal-using agents have quickly become                                ple, outcome-only recipe. We release our data,
                                                                  the most popular downstream application of                               models, and code as a strong baseline for fu-
                                                                  language models (LMs). Despite their preva-                              ture open academic work on terminal agents at
                                                                  lence, relatively little academic work has exam-                         https://github.com/hamishivi/tmax.
                                                                  ined RL-based training of these models, likely
                                                                  due to difficult benchmarks, a lack of data,                        1     Introduction
                                                                  and a lack of simple baseline recipes. We
                                                                  present TM AX, the strongest open RL recipe                         Terminal-based agentic coding products have
                                                                  for terminal agents to date, bringing open data                     quickly become incredibly popular, with models
                                                                  recipes closer to the frontier. While simple,                       expected to perform increasingly complex and
                                                                  our recipe achieves 27% on Terminal-Bench                           long-running tasks through the terminal (Anthropic,
                                                                  2.0 with only 9B parameters, outperforming                          2025; Cursor Research et al., 2026). However, ex-
                                                                  much larger models from prior work. Con-                            isting academic work has largely focused on bug-
                                                                  cretely, we generate data using a novel taxon-
                                                                                                                                      fixing setups (Jimenez et al., 2024) or relatively
                                                                  omy, combining difficulty control, personas,
                                                                  and verifier diversification, which allows us                       simple terminal tasks (Team, 2025; Gandhi et al.,
                                                                  to cheaply generate large amounts of terminal                       2025) as opposed to complex, long-horizon termi-
                                                                  environments for RL and SFT training. We                            nal tasks as exemplified by Terminal-Bench (Mer-
                                                                  open-source our terminal dataset, which is over                     rill et al., 2026). In this work, we aim to close
                                                                  2.5x larger than previously released terminal-                      this gap, providing both a large dataset of com-
                                                              * Equal contribution. Work done while HI and NL were at                 plex terminal tasks for training and a recipe for
                                            Ai2.                                                                                      training small yet powerful terminal agents using

                                                                                                                                  1
open-weight models, serving as a base for future             new skills and capabilities that generalize across
research on terminal agents. Our data generation             settings.
and model training recipes are simple yet effective,            We hope that our overall recipe and data prove
serving both as strong baselines and strong models           useful for future academic work exploring terminal
in their own right.                                          agents, as we believe this setting provides a number
   First, we introduce a new dataset, T MAX -15 K,           of unique and interesting challenges, including but
comprised of 14,600 reinforcement learning (RL)              not limited to training stability, dealing with com-
environment instances varying in their difficulty,           plex tool call setups, harness improvements, and
domain, and skills required. We generate our                 further improving performance on more difficult
data through a powerful yet simple synthetic data            downstream tasks. Concretely, our contributions
pipeline, relying on a strong frontier model to gen-         are:
erate useful environments for us. As opposed to
prior work, we explicitly control and increase the               • We introduce T MAX -15 K, a dataset of 14,600
difficulty of our tasks, and go beyond simple binary               RL environment instances, over 2.5x larger
correctness checks to continuously-valued rewards                  than prior terminal datasets.
for certain tasks. T MAX -15 K is over 2.5x larger
than the second largest terminal dataset (that                   • We train open-weight terminal agents and
releases full environment data), and is signifi-                   achieve state-of-the-art performance among
cantly harder than most prior work as judged by                    open models under 30B parameters under de-
Gemini-3-Flash-Preview pass rates.                                 fault Terminal-Bench settings. Our best 9B
   Given this data, we then explore how to per-                    model reaches 27% on Terminal-Bench 2.0.
form post-training of terminal agents with a focus
                                                                 • We provide a simple, reproducible open RL
on RL training. Despite multiple works on gen-
                                                                   recipe for achieving this performance and pub-
erating terminal agent data (Wu et al., 2026; Zhu
                                                                   licly release all elements required for train-
et al., 2026), relatively little work has explored the
                                                                   ing our models. Our recipe outperforms prior
RL training side. We develop and train models
                                                                   open RL recipes such as Endless Terminals
in a fully asynchronous RL infrastructure based
                                                                   and OpenThinker-Agent.
on open-instruct (Lambert et al., 2025). We find
naive GRPO struggles to remain stable in long-                   • We show that terminal-based RL training can
horizon, agentic scenarios, and so train our models                generalize non-trivially across harnesses and
using Divergence Proximal Policy Optimization                      tasks, providing strong evidence our training
(DPPO) (Qi et al., 2026) with an FP32 LM head                      teaches the model powerful new capabilities.
and a large group size to improve stability. Our
best 9B model achieves 27% on Terminal-Bench                   We additionally publicly release code