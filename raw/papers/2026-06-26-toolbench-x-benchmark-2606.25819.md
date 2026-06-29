---
source_url: https://arxiv.org/abs/2606.25819
ingested: 2026-06-26
sha256: 6dba699ada3bdad3a2de9daac50003b32b9e48774894a77950d4cce639035110
---
                                                        Beyond Function Calling: Benchmarking Tool-Using Agents under
                                                                        Tool-Environment Unreliability

                                                                                     Yang Tian∗ , Zhengpeng Shi∗ , Bo Zhao
                                                                                       School of AI, Shanghai Jiao Tong University




                                                                       Abstract                                returned values may be incomplete, non-canonical, or se-




arXiv:2606.25819v1 [cs.CL] 24 Jun 2026
                                           Large language models are increasingly deployed as agents           mantically shifted; and different tools may provide conflict-
                                           that solve tasks by interacting with external tool environments.    ing evidence. Even when a correct answer remains reachable,
                                           Although recent tool-use benchmarks increasingly cover com-         an agent may fail by trusting a suspicious intermediate re-
                                           plex task settings, they still largely assume clean, stable, and    sult, retrying the wrong call, inventing missing arguments, or
                                           trustworthy tool environments, leaving tool-environment un-         finishing before the required evidence is complete. These fail-
                                           reliability insufficiently examined. We introduce ToolBench-        ures are not captured well by evaluations that assume clean
                                           X, a benchmark for evaluating agents under recoverable relia-       tool contracts and focus primarily on whether the model se-
                                           bility hazards. ToolBench-X contains executable multi-step          lected the right function with the right arguments.
                                           tasks across diverse domains and sequential, parallel, and
                                           mixed workflows, each paired with deterministic tools and a            This work posits that advancing the evaluation of tool-
                                           canonical final answer for automatic evaluation. Starting from      using agents requires moving beyond mere function invoca-
                                           clean tool environments, ToolBench-X injects five structured        tion: from assessing isolated call correctness to evaluating
                                           hazard types: Specification Drift, Invocation Error, Execu-         robust task completion under conditions of structured tool
                                           tion Failure, Output Drift, and Cross-source Conflict. Cru-         uncertainty. A competent tool agent should not only execute
                                           cially, each injected instance remains solvable through at least    tools correctly but also recognize when tool outputs are un-
                                           one valid recovery path, such as retrying, fallback, verifi-        reliable, validate incomplete or contradictory results, recover
                                           cation, or cross-checking. Experiments reveal a substantial         from runtime errors, and ultimately generate a benchmark-
                                           reliability gap: agents that perform well with reliable tools       consistent final answer. In other words, evaluating tool use
                                           often fail under recoverable hazards. Further analysis shows
                                                                                                               should go beyond asking “Can the model invoke the tool?”
                                           that failures are driven less by tool-use volume or inference
                                           budget than by limited hazard diagnosis and ineffective re-         to also consider “Can the model accomplish the task when
                                           covery. Targeted recovery hints recover many failed tasks,          the tool environment is uncertain?”
                                           while test-time scaling yields more limited gains. These re-           To investigate this question, we introduce a challenging
                                           sults suggest that tool-use evaluation should move beyond           benchmark for evaluating tool-using agents under struc-
                                           function-call accuracy toward task completion under unre-           tured Reliability Hazards. The benchmark defines executable
                                           liable tool environments. The code and data is available at         multi-step tool tasks spanning sequential, parallel, and mixed
                                           https://github.com/Foreverskyou/ToolBench-X.                        workflows. Each task is associated with Python tools and a
                                                                                                               canonical final answer, enabling automatic execution and
                                                                   Introduction                                precise exact-match evaluation. We then convert previously
                                         Large language models (LLMs) are advancing at an unprece-             successful baseline tools into uncertainty-injected versions
                                         dented pace, and the development of agents built upon them            according to a controlled taxonomy of five uncertainty
                                         has emerged as a promising direction in AI research (Liu et al.       types: specification uncertainty, invocation uncertainty, exe-
                                         2024; Yang et al. 2025). These agents interact with the real          cution uncertainty, output uncertainty, and cross-source un-
                                         world through diverse tools, thereby unlocking novel oppor-           certainty. These categories encompass common real-world
                                         tunities for practical applications. Consequently, establishing       failure modes, including contract drift, argument ambiguity,
                                         robust benchmarks to reliably evaluate the tool-use capabil-          service failures, schema or surface-form drift, and conflicting
                                         ities of LLMs has become increasingly critical (Huang et al.          evidence across multiple tools.
                                         2024a; Du, Wei, and Zhang 2024b; Yao et al. 2024; Guo                    A central design principle of our framework is that in-
                                         et al. 2025).                                                         jected failures should be both disruptive and recoverable.
                                            However, correct function calling is only a necessary con-         Consequently, we avoid merely corrupting tools to create un-
                                         dition for reliable tool use. In real-world systems, tools are        solvable tasks. Instead, each injected task maintains at least
                                         rarely perfectly specified, perfectly stable, or perfectly trust-     one viable recovery path, which may involve retrying, fall-
                                         worthy. API documentation may become stale; expected                  back strategies, cross-checking, normalization, or evidence
                                         fields may be renamed or wrapped; services may timeout;               verification. Our experiments indicate that executing tasks in
                                            ∗
                                                These authors contributed equally.
                                                                                                               environments subject to reliability hazards is challenging: all
                                              #            #        Specification   Invocation   Execution   Output   Cross-Source   Sequential   Parallel    Mixed
 Benchmark
                                            Tasks        Tools         Drift          Errors      Failures    Drift     Conflict      Tool-Use    Tool-Use   Tool-Use
 ToolBench-X                                1106         4956            ✓              ✓            ✓         ✓           ✓             ✓           ✓          ✓
 BFCL v3 (Patil et al. 2025)                1,000         N/R            ×              ×            ×         ×           ×             ×           ✓          ×
 BFCL v2 (Patil et al. 2025)                2,251         N/R            ×              ×            ×         ×           ×             ×           ✓          ×
 BFCL v1 (Patil et al. 2025)                2,000         N/R            ×              ×            ×         ×           ×             ×           ✓          ×
 ToolBench (Qin et al. 2024)               126,486    16,464 APIs        ×              ×            ×         ×           ×             ✓           ×          ×
 AnyToolBench (Du, Wei, and Zhang 2024a)     400     >16,000 APIs        ×              ×            ×         ×           ×             ✓           ×          ×
 τ 2 -bench (Barres et al. 2025)             279           48            ×              ×            ×         ×           ×             ✓           ×          ×
 τ -bench (Yao et al. 2024)                  165          28             ×              ×   