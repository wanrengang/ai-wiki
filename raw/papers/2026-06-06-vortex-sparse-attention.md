---
source_url: https://arxiv.org/abs/2606.06453
ingested: 2026-06-06
sha256: 4fe61028720c334ca1fe8195fcae3992dacfc03bd3d55d7917f63eec1fc81a16
---
                                            Vortex: Efficient and Programmable
                                            Sparse Attention Serving for AI Agents
                                            Zhuoming Chen1 , Xinrui Zhong2 , Qilong Feng1 , Ranajoy Sadhukhan1 , Yang Zhou1 , Michael
                                            Qizhe Shieh3 , Zhihao Jia1 , Beidi Chen1
                                            zhuominc@andrew.cmu.edu, Xinrui.Zhong@rice.edu, {qilongf, rsadhukh,
                                            yangzhuo6}@andrew.cmu.edu, michaelshieh@nus.edu.sg, {zhihaoj2, beidic}@andrew.cmu.edu
                                            1
                                              Carnegie Mellon University
                                            2
                                              Rice University
                                            3
                                              National University of Singapore
                                             Sparse attention is becoming increasingly important for serving large language models (LLMs) as
                                            generation lengths continue to grow. However, deploying and evaluating new sparse attention algorithms




arXiv:2606.06453v1 [cs.AI] 4 Jun 2026
                                            at scale remains highly engineering-intensive, slowing both human researchers and AI agents in
                                            exploring the sparse attention design. To address this challenge, we present Vortex, a system that
                                            combines a Python-embedded frontend language atop a page-centric tensor abstraction for expressing
                                            a broad range of sparse attention algorithms, with an efficient backend tightly integrated into mod-
                                            ern LLM serving stacks. Vortex enables rapid prototyping, deployment, and evaluation of sparse
                                            attention algorithms, effectively translating their theoretical efficiency gains into real-world throughput
                                            improvements. As a result, Vortex substantially accelerates the design and iteration of sparse attention
                                            algorithms. First, AI agents use Vortex to automatically generate and refine diverse algorithms, the
                                            best reaching up to 3.46× higher throughput than full attention while preserving accuracy. Second,
                                            Vortex extends sparse attention to emerging architectures and very large models that are otherwise
                                            hard to experiment with, reaching up to 4.7× higher throughput on the MLA-based GLM-4.7-Flash and
                                            1.37× on the 229B-parameter MiniMax-M2.7 on NVIDIA B200 GPUs.
                                                                     Github: https://github.com/Infini-AI-Lab/vortex_torch
                                                                    Website: https://infini-ai-lab.github.io/vortex_torch/
                                                                   Docs: https://infini-ai-lab.github.io/vortex_torch/docs



                                                                                                            42
                                                                                                            40
                                                                                                            38


                                                                                                  mean@16 (%)
                                                                                                            36
                                                                                                            34
                                                                                                            32            AI agent generated
                                                                                                                          block_topk
                                                                                                            30
                                                                                                                          quest
                                                                                                            28            Full Attention
                                                                                                            26
                                                                                                                   4000          6000          8000   10000   12000
                                                                                                                               Throughput (tokens/sec)
                                                     (a) Agentic Workflow with Vortex                            (b) Agent-generated sparse attention

                                        Figure 1 (a). A workflow to study sparse attention algorithms using Vortex. Vortex bridges sparse
                                        attention research and real-world serving systems by enabling easy programming and efficient inference for fast iteration
                                        and large-scale validation. (b). Performance of agent-generated sparse attention algorithms on NVIDIA
                                        H200 SXM. Each point represents one sparse attention generated or optimized by AI agents using Vortex. By
                                        combining programmable abstractions with efficient execution in serving systems, Vortex enables large-scale autonomous
                                        experimentation and iterative optimization. The best generated sparse attention achieves up to 3.46× higher throughput
                                        than full attention while preserving accuracy.




                                                                                                         1
1      Introduction
Sparse attention is emerging as a fundamental technique for reducing inference costs in large language models
(LLMs), driven by the rapid growth of generation lengths in applications such as reasoning (Guo et al., 2025),
agentic systems (Wang et al., 2024), and reinforcement learning (Guo et al., 2025; Schulman et al., 2017). In
these workloads, key-value (KV) cache movement during decoding has become the primary system bottleneck.
As a result, sparse attention is being adopted both as a core architectural component in state-of-the-art models
such as DeepSeek (Liu et al., 2025; Yuan et al., 2025; DeepSeek-AI, 2026) and GLM-5.1 (Zeng et al., 2025),
and as a drop-in optimization for pretrained models (Chen et al., 2024; Tang et al., 2024; Wang et al., 2026;
Sun et al., 2024; Singhania et al., 2024; Zhang et al., 2023; Yang et al., 2025b; Lin et al., 2024; Xiao et al., 2024;
Zhao et al., 2025; Cai et al., 2024; Yang et al., 2024a). To accelerate progress in sparse attention algorithms,
reducing implementation complexity while maintaining high serving efficiency is becoming critical.
Deploying and validating new sparse attention algorithms
at scale while achieving end-to-end speedups remains                               Physical View

highly challenging and engineering-intensive, substan-
                                                                                                         Paged Layout

                                                                                                  𝑝    𝑝    𝑝      𝑝0    𝑝  1   2 𝑝 3   4       5

tially slowing both human researchers and emerg-                      Batch Layout
                                                                                             Sequence                 Page Index

ing AI agents in exploring the sparse attention design                                           𝑰
                                                                                                 𝑰
                                                                                                                    𝟎
                                                                                                                        [ 0, 2, 5 ]
                                                                                                                        [ 1, 3, 4 ]
space. The core difficulty arises from modern LLM serv-
                                                                                                                    𝟏

                                                                                                 𝑰                  𝟐   [ 0, 3, 5 ]


ing systems that adopt paged attention to reduce mem-                              Logical View

ory fragmentation, in which the KV cache is stored in a             𝒙                  𝟎
                                                                                               𝒙      𝑝     𝑝   𝟎  𝑝   ( from 𝐼 = [0, 2, 5] )
                                                                                                                        0       2   5       0



physically non-contiguous, block-sparse layout accessed             𝒙                  𝟏
                                                                                               𝒙      𝑝     𝑝   𝟏  𝑝   ( from 𝐼 = [1, 3, 4] )
                                                                                                                        1       3   4       1



through indirect addressing (Kwon et al., 2023). This               𝒙                  𝟐
                                                                                               𝒙      𝑝     𝑝   𝟐  𝑝   (0
                                                                                                                          from 𝐼 = [0, 3, 5] )
                                                                                                                                3   5       2



breaks the memory assumptions underlying existing ten-
sor frameworks such as PyTorch (Paszke et al., 2019)
for batch (contiguous) tensor layouts (illustrated in Fig-   Figure 2 Illustration of paged layouts.
ure 2). Consequently, sparse attention algorithms are
difficult to express within existing tensor abstractions. Despite substantial progress toward addressing these
challenges, three key gaps remain.
1. Difficult to Generalize to Dynamic Sparsity. Prior work (e.g., FlashInfer (Ye et al., 2024a),
FlexAttention (Dong et al., 2024)) generalized the attention operator to tensors with a paged layout. Thus,
they can optimize sparse attention kernels once the sparsity patterns (i.e., the set of KV pairs each query
attends to) are known. While these approaches achieve substantial speedups for static sparse attention, they
do not naturally extend to dynamic sparse attention, which proved to be more accurate. In dynamic settings,
determining the sparsity pattern on the fly is a central component of the algorithm and incurs computational
overhead that exceeds that of the attention operator.
2. Lack of Programmability. Existing LLM serving systems (Zheng et al., 2023; Kwon et al., 2023;
Contributors, 2023) support several sparse attention methods but provide limited programmability for integrating
new algorithms. As a result, adding a new variant can require substantial engineering effort. For example,
implementing Double Sparse (Yang et al., 2024b) in SGLang (Zheng et al., 2023) requires roughly 2000
lines of code, much of which reimplements operators such as GeMM, Reduce, and Top-K over paged tensors.
This significantly hinders both researchers and AI agents (Sharma, 2025; Novikov et al., 2025) from rapidly
prototyping and iterating on new sparse attention ideas.
3. Incompatibility with Evolving Serving Systems. Many sparse attention methods rely on custom
kernels (Sun et al., 2024; Tang et al., 2024; Lin et al., 2025; Chen et al., 2024), but remain incompatible
with key components of modern LLM serving stacks, including paged attention (Kwon et al., 2023), prefix
caching (Zheng et al., 2023), and rapidly evolving attention backends (Ye et al., 2024a; Zadouri et al., 2026;
Shah et al., 2024; Jiashi Li, 2025). As a result, their theoretical speedups often fail to translate into practical
end-to-end gains. For example, Quest’s original implementation is 44.4× slower than SGLang full attention,
taking nearly two hours to evaluate Llama 3.1-8B on AMC23, despite optimized sparse kernels. These results
suggest that evaluating sparse attention on long-generation workloads is impractical without tight integration
with modern serving systems, even during prototyping and iteration.
An ideal framework should allow AI agents to express sparse attention mechanisms by specifying what sparsity
pattern to apply and how attention is computed, while abstracting away from low-level tensor layouts and


                                                                      2
memory-management details. At the same time, the framework should transparently optimize execution
and remain fully compatible with existing LLM serving infrastructures, enabling researchers to obtain rapid
and realistic performance feedback.
A natural approach toward such a framework is to implement each sparse attention variant as a dedicated
custom operator. However, this strategy fundamentally lacks compositionality: every new sparsity pattern
or algorithm requires a separate implementation, making the system difficult to extend and unable to scale
with the rapidly growing diversity of sparse attention designs.
Inspired by abstractions for sparse tensor linear algebra (Abdelfattah et al., 2024; Duff et al., 2002; Ye et al.,
2023, 2024a), which provide systematic representations and optimizations for sparse computation, we propose
vTensor, a page-centric tensor system. vTensor generalizes sparse tensor computation to a paged layout,
enabling sparse attention algorithms to be expressed in a flexible and composable manner without exposing
low-level memory layouts or addressing details. However, leveraging vTensor to program sparse attention
and achieve speedups for LLM serving requires overcoming several technical challenges.
• Simple and Modular Programming Interface. The framework should provide an intuitive tensor view
  that hides low-level layout details and reduces user complexity. Sparse attention algorithms should also be
  expressed as modular components, even when they involve computation across stages of the serving pipeline.
• Efficient Execution on Paged Layouts. Supporting heterogeneous tensor layouts makes kernel implemen-
  tation challenging. Operators must execute efficiently on non-contiguous memory layouts while minimizing
  gather operations and redundant data movement.
To address these challenges, we propose Vortex, a system for efficient and programmable sparse attention
serving. As shown in Figure 1a, Vortex consists of three components: (1) a frontend language (vFlow)
embedded in Python for expressing sparse attention algorithms; (2) an interpreter that translates vFlow
programs to executable vTensor operators; and (3) an execution backend that integrates seamlessly
with existing serving systems.
• In Section 3.1, we introduce the vFlow programming model through an example based on block top-k
  attention, and demonstrate its expressiveness across a wide range of sparse attention algorithms in Section 3.2.
• In Section 4.1, we present the vTensor abstraction for paged tensor computation. We then describe
  in Section 4.2 how vFlow programs are compiled and executed efficiently as vTensor programs within
  modern serving systems.
• In Section 5, we present three key optimizations in Vortex: a workload planner for balanced GPU thread-
  block scheduling, kernel fusion to reduce intermediate memory traffic (see Section 5.1), and stochastic
  optimizations for radix top-k, a critical operator in sparse attention (see Section 5.2). In Section 5.3, we
  present Vortex’s compatibility with existing optimized attention backends, including GQA and MLA.
In Section 6.1, we first demonstrate that Vortex enables rapid innovation and iteration of sparse attention
algorithms with AI agents, as shown in Figure 1b. In Section 6.1.1, Claude Code and Codex are prompted
to generate a diverse set of sparse attention algorithms that achieve strong accuracy-throughput trade-offs.
In Section 6.1.2, an 18-hour autonomous optimization loop improves throughput by up to 3.46× over full
attention while preserving accuracy. In Section 6.4, we then evaluate the serving efficiency of Vortex.
Compared to SGLang, Vortex achieves up to 3.60× and 2.98× throughput improvements over full attention
for block top-k and Quest, respectively, while maintaining accuracy. In terms of user latency, Vortex reduces
P95 latency by up to 11.7× for block top-k attention and 12.8× for Quest under long-input workloads, all
evaluated on NVIDIA H200 SXM.
Beyond these, Vortex readily extends to new attention architectures and to very large models that are
otherwise hard to even experiment with. On the MLA-based GLM-4.7-Flash, running on an NVIDIA B200
(Section 6.2), a rope-aware sparse attention we design in vFlow achieves up to a 4.7× speedup over full
attention, and on the 229B-parameter MiniMax-M2.7 served across four NVIDIA B200 GPUs (Section 6.4),
block top-k achieves up to a 1.37× speedup and even exceeds full-attention accuracy. These results show that
Vortex not only delivers high-performance sparse attention serving but also provides a scalable step towards
automated research and optimization of sparse attention by AI agents.




                                                        3
2     Related Work
LLM Serving Systems. A large body of work has improved the efficiency of LLM serving (Yu et al., 2022;
Kwon et al., 2023; Patel et al., 2024; Agrawal et al., 2024; Zhong et al., 2024; Holmes et al., 2024; NVIDIA; Qin
et al., 2024; Zheng et al., 2023; Sheng et al., 2023; Mei et al., 2024; Oliaro et al., 2024; Miao et al., 2023b,a;
Contributors, 2023). Orca (Yu et al., 2022) introduced continuous batching. vLLM addresses the memory
fragmentation bottleneck with paged-attention. SGLang (Zheng et al., 2023) focuses on prefix caching, which
reuses KV cache across repeated prefixes to reduce redundant computation.
Sparse Attention. Sparse attention for LLMs can be broadly divided into training-time and inference-time
approaches. Training-time methods incorporate sparsity directly into model architectures during pre-training
(DeepSeek-AI, 2026; Liu et al., 2025; Yuan et al., 2025; Lu et al., 2025; Huang et al., 2025; Chen et al., 2021;
Zandieh et al., 2023). Inference-time methods instead accelerate pretrained dense models with little or no
retraining. These approaches are commonly categorized as static or dynamic. Static sparse attention (Xiao et al.,
2023; Agarwal et al., 2025; Xiaomi et al., 2025; Child et al., 2019; Ding et al., 2023) applies predefined sparsity
patterns independent of input content, typically based on token positions. Dynamic sparse attention (Tang
et al., 2024; Xiao et al., 2024; Chen et al., 2024; Sun et al., 2024; Singhania et al., 2024; Lin et al., 2025;
Zhao et al., 2025; Gao et al., 2024; Zhang et al., 2023, 2024; Ge et al., 2023; Kim et al., 2025; Li et al., 2024;
Adnan et al., 2024) instead constructs input-dependent sparsity patterns by selecting KV entries according
to the current query, hidden states, or content relevance.
System Optimizations for Attention. Prior systems such as FlashAttention (Dao et al., 2022b; Dao,
2023; Dao et al., 2022a; Zadouri et al., 2026), Flash-Decoding (Ye et al., 2024b; Hong et al., 2024), and
SlimAttention (He et al., 2024) accelerate attention through improved hardware utilization and parallelism.
Frameworks including FlashInfer (Ye et al., 2024a), FlexAttention (Dong et al., 2024) further expose sparse
attention interfaces based on KV block indices or masks for paged attention. However, these systems generally
assume the sparse KV indices are already available, leaving the challenge of efficiently constructing them
unresolved. More recent systems, such as LServe (Yang et al., 2025b) and SparseServe (Zhou et al., 2025),
explore end-to-end sparse attention optimization but focus on specific algorithms or deployment settings,
including Quest (Tang et al., 2024) and block top-k attention under memory-constrained workloads.
Currently, Vortex focuses on the decoding phase, where KV memory is the primary bottleneck, and leaves
the prefilling phase for future work.


3     Programming Model: vFlow
In this section, we present the programming model of Vortex, namely vFlow, a unified abstraction for
expressing sparse attention algorithms. We begin in Section 3.1 with a concrete case study on block top-k
attention to illustrate the core design principles of vFlow. In particular, vFlow adopts a single-request mental
model, treating tensors as batch size 1, while exposing flexible and composable primitives for constructing
sparse attention mechanisms. We then show in Section 3.2 how this abstraction naturally generalizes to a
broad range of sparse attention algorithms.

3.1    A Running Example: Block Top-k Attention
Let q ∈ Rg×d denote the query, and let K ∈ Rn×d and V ∈ Rn×d denote the key and value caches, respectively.
We partition the sequence into B = ⌊n/b⌋ non-overlapping blocks of size b. The mathematical formulation
for block top-k attention (for a single key value head) is defined in Figure 3 and the corresponding vFlow
implementation is presented in Figure 4, where primitives by vFlow are shown in purple.
This formulation naturally decomposes into two stages with distinct computational characteristics:
(1) Query-independent preprocessing. Equation (1) computes block-level key summaries k  ej by averaging
keys within each block. These summaries depend only on the key cache K and are independent of the query,
and thus can be precomputed once during cache construction and reused across decoding steps. This stage
corresponds to forward_cache() in Figure 4, where vFlow presents a logical view of c["centroids"] as
a contiguous tensor of shape [1, B, d].


                                                        4
                                                       
      ej = mean K[jb:(j+1)b−1] , dim=0 , j ∈ [0, B)
      k                                                        (1)
                                                                    class BlockTopK ( vFlow ) :
      s
      ej = mean      q kjT , dim=0
                       e                    , j ∈ [0, B)       (2)       def f o r w a r d _ c a c h e ( c ) :
                                                          
                                i
                               j k                                           c [ " centroids " ] = Mean ( c [ " k " ] , dim =1)
       I=       i ∈ [0, n) |         ∈ arg top-k s
                                                 ej            (3)
                                b            j∈[0,B)
                                                                        def f o r w a r d _ i n d e x e r (q , c ) :
                           qKIT                                               s = GeMM ( c [ " centroids " ] , q . T )
       o = softmax          √           VI                     (4)            i = topK ( Mean (s , dim =2) , dim =1)
                              d
                                                                              return attn (q , c [ " k " ] , c [ " v " ] , i )


   Figure 3 Formulations of block top-k attention.                           Figure 4 Block top-k attention in vFlow.

(2) Query-dependent execution. Equations (2) to (4) define the dynamic computation performed for
each incoming query q. Specifically, the model (i) computes block relevance scores sej , (ii) selects the top-k
blocks, and (iii) performs attention over the corresponding tokens. This stage must be executed per token, as
both the selected index set I and the output o depend on the current query. This stage is implemented as
forward_indexer(), where vFlow exposes logical contiguous views for all tensors: c["centroids"] as [1,
B, d], c["k"] and c["v"] as [1, B, b, d], and q as [1, g, d]. Under this logical view, users can easily
reason about intermediate tensor shapes; for example, s ∈ R1×B×g and i ∈ R1×1×k .

3.2    Generalizability
The vFlow model is highly general and can express a wide range of sparse attention algorithms. At a high
level, its two-stage decomposition mirrors the standard paradigm in retrieval and similarity search (Douze
et al., 2025; Jegou et al., 2010; Malkov and Yashunin, 2018; Shrivastava and Li, 2014): query-independent
auxiliary structures (e.g., summaries, centroids) are constructed offline to enable efficient query-time selection.
We further demonstrate the expressiveness of vFlow by answering the following questions.
Q1. Can vFlow easily express more complicated dynamic sparse attention algorithms? Yes. vFlow
can naturally express a wide range of dynamic sparse attention algorithms by composing a small set of reusable
operators. As shown in the examples (DoubleSparse (Yang et al., 2024b), BlockTopK_NSA (Yuan et al., 2025),
Quest (Tang et al., 2024), and H2O (Zhang et al., 2023), in Figures 5 to 8.), different methods can be implemented
by combining common building blocks such as GeMM, Softmax, Top-K, Gather, and reduction operators (the
full operator set is listed in Table 1). This compositionality is enabled by vTensor (see Section 4.1), which
provides a unified interface for logical tensors while handling physical layout transparently.
Q2. Can intermediate computation results of forward_indexer() be cached? Yes. vFlow enables
caching of intermediate results by allowing operator outputs to be stored in a named cache, provided the user
explicitly declares the cache in forward_cache(). A concrete example is H2O, where c["acc"] accumulates
attention scores across decoding steps. This design facilitates the implementation of algorithms that require
runtime statistics (Zhang et al., 2023; Yang et al., 2024a; Bai et al., 2026).
Q3. Can vFlow extend to static sparse attention?
Yes. Static sparse attention can be naturally expressed in vFlow by constructing index sets that depend only
on sequence positions rather than input content. For example, one can obtain the current sequence length via
B = c["k"].shape[1] and derive the token position accordingly. The sparsity pattern can then be explicitly
specified by constructing the index set i (e.g., i = List[...]), independent of the query values.
class DoubleSparse ( vFlow ) :
    def f o r w a r d _ c a c h e ( c ) :                                class BlockTopK_NSA ( vFlow ) :
        c [ " channel " ]= Gather ( c [ " k " ])                             def f o r w a r d _ c a c h e ( c ) :
    def f o r w a r d _ i n d e x e r (q , c ) :                                 c [ " centroids " ]= Mean ( c [ " k " ])
        s = GeMM ( c [ " channel " ] ,                                       def f o r w a r d _ i n d e x e r (q , c ) :
        Gather ( q ) . T )                                                       s = Softmax ( c [ " centroids " ] @q . T )
        i = topK ( Sum ( s ) )                                                   i = topK ( Sum ( Conv ( s ) )


                  Figure 5 Double Sparse                                            Figure 6 NSA (block top-k part)




                                                                     5
class Quest ( vFlow ) :                                      class H2O ( vFlow ) :
    def f o r w a r d _ c a c h e ( c ) :                        def f o r w a r d _ c a c h e ( c ) :
        c [ " max " ]= Max ( c [ " k " ])                            c [ " acc " ]=0
        c [ " min " ]= Min ( c [ " k " ])                        def f o r w a r d _ i n d e x e r (q , c ) :
    def f o r w a r d _ i n d e x e r (q , c ) :                     s = Softmax ( GeMM ( c [ " k " ] , q . T ) )
        s = Maximum ( q * c [ " max " ] ,                            c [ " acc " ]+= s
                            q * c [ " min " ])                       i = topK ( Sum ( s ) )
        i = topK ( Max ( Sum ( s ) ) )                               Mask (~ i , c [ " acc " ] , - inf )


                         Figure 7 Quest                                             Figure 8 H2 O



4     Interpretation: vTensor
The vFlow programs provide a simple yet effective logical tensor abstraction that allows programmers to
assume tensors are contiguous and processed with an effective batch size of 1. However, tensors in real LLM
serving systems are often stored using more complex layouts to support continuous batching and paged KV
caches (Appendix A). Thus, we need an underlying tensor system that explicitly accounts for paged layouts
to interpret the semantics of vFlow into executable programs. To ensure the flexibility and simplicity of
vFlow, the tensor system must satisfy two key properties:
Compositional. Complex algorithms should be constructed by composing a small set of primitive tensor
operators, rather than relying on highly specialized monolithic kernels. This design improves modularity,
extensibility, and portability across different serving workloads.
Self-Contained. The outputs of tensor operators should remain in formats directly consumable by subsequent
operators. In particular, intermediate resu