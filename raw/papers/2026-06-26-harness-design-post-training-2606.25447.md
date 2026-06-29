---
source_url: https://arxiv.org/abs/2606.25447
ingested: 2026-06-26
sha256: a5346dc193d016b5e9b745ae0af4d93627f63d4df6ffbf9afdfbb0f735d0d854
---
                                                 The Interplay of Harness Design and Post-Training in LLM Agents

                                                           Kyungmin Kim1,∗ , Youngbin Choi1,∗ , Seoyeon Lee1 , Suhyeon Jun2 ,
                                                                          Dongwoo Kim1,2,† , Sangdon Park1,2,†
                                                                  1
                                                                    Graduate School of Artificial Intelligence, POSTECH,
                                                             2
                                                               Department of Computer Science and Engineering, POSTECH,
                                                     {kkm959595, choi.youngbin, seoyeon26, suhyeonjun, dongwoo.kim, sangdon}@postech.ac.kr




                                                                   Abstract                             ing efforts to post-train pretrained LLMs for effec-
                                                                                                        tive tool use (Feng et al., 2025).
                                             Tool-integrated LLM agents are often wrapped




arXiv:2606.25447v1 [cs.LG] 24 Jun 2026
                                                                                                           A factor that is often overlooked in this line of
                                             within a harness: the scaffolding that deter-
                                             mines which tools are exposed, how they are
                                                                                                        work is the harness: the code that wraps the LLM,
                                             described, and what auxiliary information ac-              determining what is stored, retrieved, and presented
                                             companies each per-step observation. While                 to it, and what is done with its outputs (Wang et al.,
                                             agents are routinely post-trained, this scaffold-          2025; Anthropic, 2026; OpenAI, 2026). While the
                                             ing is typically treated as a fixed engineering            harness is, in principle, an engineering detail, in
                                             detail, with design effort limited to the training-        practice it is a major determinant of overall perfor-
                                             free regime. Moreover, existing post-training              mance: even with the same task and model, two
                                             algorithms assume a static environment, even
                                                                                                        harnesses that differ only in how they present aux-
                                             though tool environments and tasks often shift
                                             upon deployment. To address this gap, we                   iliary information can produce drastically different
                                             extend ALFWorld (i) to treat the harness as a              success rates (Yang et al., 2024; Badertdinov et al.,
                                             controllable design dimension and (ii) to sup-             2025). For this reason, harness design has attracted
                                             port evaluation under task and tool environ-               increasing attention, ranging from manually cu-
                                             ment shifts. Building on this, we systemati-               rated prompts (Hong et al., 2024; Antoniades et al.,
                                             cally analyze how the harness design influences            2025; Lin et al., 2026) to recent attempts at au-
                                             post-training in both in-distribution and out-of-
                                                                                                        tomating the design itself (Lee et al., 2026; Shang
                                             distribution (OOD) settings. We empirically
                                             show that harness-aware post-training not only
                                                                                                        et al., 2025). All of these efforts, however, are
                                             improves in-distribution performance but also              limited to a training-free regime, treating the har-
                                             enables agents to robustly adapt to OOD set-               ness as a way to elicit better performance, typically
                                             tings. Under a harness with minimal design ef-             from a closed-source model. How harness design
                                             fort, post-training suffers a drastic performance          interacts with post-training, in contrast, remains
                                             drop under stronger tool environment shifts,               largely unexamined.
                                             further highlighting the importance of harness-               In parallel, most existing post-training ap-
                                             aware post-training under such shifts.
                                                                                                        proaches for tool-integrated agents implicitly as-
                                         1   Introduction                                               sume a static deployment scenario, in which both
                                                                                                        the task distribution and the tool environment, de-
                                         LLM agents are designed to solve complex prob-                 fined as the set of tools and their invocation pro-
                                         lems that often require multi-step reasoning (Yao              tocols, remain fixed (Jin et al., 2025; Mai et al.,
                                         et al., 2023; Shinn et al., 2023; Wang et al., 2024;           2025; Xue et al., 2026; Qian et al., 2025; Feng
                                         Yang et al., 2024; Wu et al., 2025). Among them,               et al., 2026; Jiang et al., 2025; Cheng et al., 2025).
                                         tool-integrated agentic systems extend LLM ca-                 In practice, this assumption is often violated: real-
                                         pabilities by proactively interacting with environ-            world deployments expose agents to shifts in both
                                         ments through predefined tools (Schick et al., 2023;           the task distribution and the tool environment. A
                                         Qin et al., 2024; Patil et al., 2024; Liu et al., 2024).       first source of shift arises from changes in the task
                                         Accordingly, performance depends not only on pro-              distribution, a standard out-of-distribution (OOD)
                                         ducing correct final outputs but also on appropri-             problem at the input level (Koh et al., 2021; Yuan
                                         ately invoking tools, which has motivated increas-             et al., 2023; Yang et al., 2023). For example, an
                                             ∗
                                                 Equal contribution.                                    agent originally designed for workload schedul-
                                             †
                                                 Co-advising.                                           ing may later be repurposed for itinerary planning,

                                                                                                    1
       State 𝒔𝒕 in ALFWorld                     Harness Building Block                     Zero-shot                                              𝒒𝒕𝒆𝒔𝒕 : Put two toilet paper in toilet

                                                                                                                                                        Go(“toilet 1”)
                                             ② 𝒑𝒔𝒉𝒐𝒓𝒕 : short tool description
   Text action description 𝒑                 Go: Move to a receptacle.
                                                                                                    h-low              h-mid            h-high

                                                                                                                                                          You arrive at
                                               Parameters – receptacle (string)                                                                            toilet 1. …
                                                                                                            Agent with each harness

   ① User query 𝒒 & History 𝓣𝒕−𝟏              ③ 𝒑𝒓𝒊𝒄𝒉 : rich tool description              Post-training: in-distribution
   [User query]                               Go: … you must be at a receptacle
                                              before you can Take, Open, Move.                                   Go(“toilet 4”)                         Go(“toilet 1”)
   Put two toilet paper in toilet
   [History]
                                                                                                               Nothing happens.                           You arrive at
   go to toilet paper hanger 1               ④ Carrying:      