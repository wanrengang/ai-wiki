---
source_url: https://mp.weixin.qq.com/s/
ingested: 2026-06-19
sha256: 35a32c88ff53a69897bd3a3c064080c79aa41a2c47bdc0d2f75b2b9641effd80
---
新智元报道

 【新智元导读】

 219个词喂给AI，12小时后，一份7nm芯片版图出来了，工程师全程没碰键盘。这条芯片行业几十年没有AI走完过的路，第一次走通了。

 一套跑在云端的大模型Agent系统，收到了一段219个英文单词的需求描述。

 12小时后，它输出了一颗CPU的GDSII版图文件。整个过程，没有工程师参与过任何一个设计环节。

 虽然这颗VerCore的跑分仅相当于2011年的Intel Celeron SU2300，并不包含缓存实现，甚至还没流片。

 但真正震动EDA圈的，不是这颗CPU能跑多快，而是这12小时里发生的事：从需求理解、架构设计、代码实现、功能验证，到时序收敛、物理版图，AI自己全走完了。

 「工程师写代码」这个环节，第一次被AI整段跳过。

 https://arxiv.org/abs/2603.08716

 AI芯片设计初创公司Verkor在技术报告中披露了这项工作，他们把这套系统叫Design Conductor，它设计出来的CPU被称作VerCore。

 Design Conductor接收的是一段219个英文单词描述的需求，核心要求大意是：

 你的任务是构建VerCore，一个支持RV32I和ZMMUL的RISC-V CPU核心，实现简单的5级流水线、顺序、单发射设计，目标CoreMark分数最大化，目标时钟频率1.6GHz，使用OpenROAD flow脚本生成最终GDSII输出，使用ASAP7 PDK。

 它产出的是基于ASAP7 学术7nm PDK的tape-out ready GDSII版图文件。

 这个过程中间包括一整条传统芯片设计流程：需求分析、架构设计、RTL编码、功能验证、时序收敛、后端布局布线。

 把整条流程类比成造一栋楼，传统做法是建筑师画图、结构工程师算梁柱、监理验收、施工队布管线，几十上百号人层层接力。

 而Design Conductor就像一个总包，你给它几句话，它就把所有环节做完。

 用Verkor创始人Suresh Krishna的话说：

 让AI Agent把整个问题自己解决掉

 。

 它不是聊天机器人

 而是一个会调度工具链的Agent

 Design Conductor本身不是一个AI模型，而是一个LLM harness，一套围绕大语言模型构建的任务编排框架。

 根据论文描述，Design Conductor的架构包含核心调度模块、上下文管理器、长期记忆系统、LLM会话、工具服务器和执行环境。

 它运行在分布式云端，支持多个子Agent实例并行工作。

 设计开始时，Design Planning Agent负责需求分析、微架构和实施计划；随后Design Review Agent 对方案做「painstaking, manual」的逐场景审查。

 紧接着，Module Implementation Subagent负责实现每个模块、生成测试台，并在模块级测试通过后移交集成。

 System Integration Agent负责把所有RTL整合起来，运行端到端系统测试。

 如果出现功能错误，Root Cause Analysis Subagent负责从VCD波形里找根因、提出修复方案。

 最后，PPA Closure Subagent负责分析时序、面积、功耗问题，必要时修改RTL和后端脚本，直到满足约束。

 AI做的事，是把这些工具按正确的顺序调用起来，让它们协同跑完整个流程。

 它debug的方式

 像个有洁癖的老工程师

 论文里记录了Design Conductor自主发现并修复一个典型bug的过程，读完你会觉得，这不像AI在做事，更像一个有洁癖的资深工程师。

 测试运行时，系统发现寄存器写入出现了不匹配：预期写入x2寄存器，实际却是x5被写了错误的值。

 DC没有蒙。它先把VCD波形文件转成CSV，写了一段Python脚本，逐行提取时间戳、寄存器地址和写入数据，然后跟Spike模拟器的参考输出逐条比对。

 定位出来了：PC=0x2008处有一条JAL指令（跳转并链接），branch_taken信号已正确拉高，目标地址是0x2020。但跳转后，原本应该被清除的0x200c处的指令（AUIPC x5）没有被冲刷，继续执行，把错误的值写入了x5。

 根本原因是：流水线flush逻辑存在缺陷，跳转发生后，投机取指的指令没有被正确作废。

 DC随后生成了修复方案，改动RTL，重新跑测试，通过。

 整个过程没有人工介入。

 但这个过程并不优雅。Verkor工程副总裁David Chin对此解释道：「我们在用算力换经验。」

 换句话说，AI Agent能跑通流程，不等于它真懂这件事。它用试错的次数，补上了人类工程师靠多年积累形成的直觉。

 跑分停在2011年

 但AI打通的那条路从来没有人走完过

 VerCore最终的量化结果是：CoreMark分数3261，时钟频率1.48GHz，面积2809平方微米，使用ASAP7 7nm PDK。

 Verkor在论文里给出了一个性能参照：大致相当于2011年中期的Intel Celeron SU2300。

 CoreMark是嵌入式处理器核心的常用基准测试，专门测的是处理器核心功能，不代表桌面CPU的综合性能、缓存体系、系统吞吐量。

 此外，VerCore目前只存在于仿真中，通过Spike ISA模拟器做功能验证，版图在ASAP7学术预测性PDK上生成。ASAP7是研究用PDK，不等于台积电或三星的真实量产7nm工艺。理论上可以送往代工厂，但Verkor目前尚未实际流片。

 迭代成本断崖式下降

 但算力代价也不容忽视

 传统领先芯片设计，超过4亿美元投入，18至36个月周期，数百人团队，验证成本占总成本50%以上。

 这个门槛决定了一件事：很多本来可以存在的芯片，因为太贵而从来没有被造出来。Design Conductor想解决的，正是这个问题。

 Verkor的愿景是未来原本需要100人以上、花18至36个月才能完成一颗芯片的团队，将能够同时探索多个从概念到GDSII的设计方案，整体流片周期压缩到3至6个月。

 当然，算力代价也是真实的。设计这颗相对简单的CPU，Design Conductor消耗了「数百亿个token」。

 判断力

 仍是变革中最值钱的东西

 判断力，仍是这场变革中最值钱的东西。

 Verkor团队在论文里坦承了三个问题。

 架构决策上，AI有时会绕远路。比如时序跑不过时，它会先去想要不要把流水线加深，而有经验的工程师一眼就知道先找更简单的原因。

 代码理解上，AI会把硬件描述语言的运行逻辑当成普通程序来理解。硬件是并行的，不是顺序执行的，这个误判会让调试过程变得很低效。

 规格理解上，需求文档必须写得极度精确。少写一个CPI约束，AI可能就默默交出一颗分支处理很差的处理器。

 Verkor设想的未来团队模式，多个子团队各自从概念跑到版图，打破单一串行流程：

 5–10 名跨领域专家设定目标、审查关键决策，再由一群 Agent 承接大量 RTL、验证和后端迭代工作。

 参考资料：

 https://spectrum.ieee.org/ai-chip-design

 https://arxiv.org/abs/2603.08716

 https://verkor.io/

 https://www.tomshardware.com/tech-industry/artificial-intelligence/ai-agent-designs-a-complete-risc-v-cpu-from-a-219-word-spec-in-just-12-hours

 编辑：元宇
