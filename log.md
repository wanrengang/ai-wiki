# Wiki Log

> 持续记录所有 wiki 操作。只追加，年度轮换。
> 格式：`## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete

## [2026-05-03] create | AI Wiki initialized
- Domain: AI/ML 研究、前沿模型、AI Agent 技术
- Path: /run/media/wrg/mywork/data/ai-wiki
- Structure: SCHEMA.md, index.md, log.md, raw/{articles,papers,transcripts,assets}, entities/, concepts/, comparisons/, queries/

## [2026-05-05] create | Claude Code 会话归档系统
- 来源：微信公众号文章
- 路径：concepts/claude-code-session-archive.md
- 核心：三层架构(jsonl→md转换)、session_id双向同步、四层检索机制

## [2026-05-03] ingest | 第二批 17 个 AI 笔记（2026-03-08~2026-04-09）
- 源文件路径：/run/media/wrg/mywork/data/my-obsidian/02AI与技术/
- 处理内容：
  - AIFUT 大会干货总结
  - GLM-5.1 电商风格迁移项目实战
  - NotebookLM 20个黄金指令
  - Vercel agent-browser 浏览器自动化
  - Claude Managed Agents 深度分析
  - 企业级 Skill 构建
  - CLI工具-飞书钉钉赫尔墨斯
  - OpenClaw Temporal 集成方案
  - Superpowers Claude Code 编程工作流规范
  - 钉钉悟空 vs 龙虾 企业级 AI 选型
  - 近两周热门 OpenClaw Skills 榜单
  - 办公领域热门 OpenClaw Skills 榜单
  - Anthropic 首超 OpenAI 暴买谷歌 TPU

## [2026-06-03] ingest | Agent Behavioral Trajectory Tracking (arXiv 2606.02536)
- 来源：arXiv HTML 页面（cs.AI，2026-06-02，ICML AIWILD Workshop）
- 主题：行为轨迹追踪 — 技能文件 embedding 空间 trait 向量量化 Agent 行为变化，68 对数据 91.2% Sign 准确率，Agent-to-Agent 协议通过可信中间件实现跨 Agent 行为评估，Hermes 部署案例
- raw/papers/：2026-06-03-behavioral-trajectories-adapting-agents.md（新建）
- entities/：entities/tracking-behavioral-trajectories-arxiv-2606.02536.md（新建）
- concepts/：concepts/agent-behavioral-trajectory-tracking.md（新建）
- 跨链接：agent-behavioral-trajectory-tracking → [[harness-engineering]], [[agent-drift-multi-agent-systems]], [[hermes-agent]], [[mcp]], [[scheming-propensity-llm-agents]]
- 主动发现：从 arXiv cs.AI listing 发现，Trait Vector 方法论 + Agent-to-Agent 协议在 Agent 安全领域有独特价值，值得入库
- 总入库：1 raw + 1 entities + 1 concepts
- entities/aifut-conference.md
- entities/glmmodel-51.md
- entities/notebooklm.md
- entities/vercel-agent-browser.md
- entities/claude-managed-agents.md
- entities/dingtalk-wukong.md
- entities/dingtalk-workspace-cli.md
- entities/hermes-agent.md

## [2026-05-03] create | 8 个 Concepts 页面（第二批）
- concepts/harness-engineering.md
- concepts/enterprise-skill-architecture.md
- concepts/openclaw-skills-list.md
- concepts/superpowers-claude-code-workflow.md
- concepts/openclaw-temporal-integration.md
- concepts/feishu-lark-hermes-comparison.md
- concepts/anthropic-tpu-advantage.md
- concepts/awesome-openclaw-skills.md

## [2026-05-03] create | 1 个 Comparison 页面（第二批）
- comparisons/dingtalk-wukong-vs-openclaw.md

## [2026-05-03] update | index.md
- 添加 8 个 Entities 条目（第二批）
- 添加 8 个 Concepts 条目（第二批）
- 添加 1 个 Comparison 条目（第二批）
- 更新 Total pages: 40（累计）

## [2026-05-03] update | log.md
- 追加第二批 ingest 和 create 记录

## [2026-05-03] ingest | 第一批 17 个 AI 笔记（2026-02-05~2026-03-06）
- 源文件路径：/run/media/wrg/mywork/data/my-obsidian/02AI与技术/
- 处理内容：
  - Superpowers 技术研究报告 + 日常使用指南
  - Chrome DevTools Protocol 技术调研
  - OpenAI Frontier 企业级 AI 代理平台
  - Agent 工程化实战指南
  - AI-Agent 技术深度解析
  - OpenCLAW 近半月更新与全面技术报告
  - OpenCode 平台构建个人助手
  - nanobot 从入门到精通
  - 国内 MCP 服务器生态调研
  - Agent-Browser 赋予 AI 浏览器超能力
  - Capability-Evolver 自我进化引擎
  - GitHub AI 热门项目分析
  - GPT-5.4 技术深度研究
  - Xiaomi miclaw 调研报告

## [2026-05-03] create | 12 个 Entities 页面（第一批）
- entities/superpowers.md
- entities/chrome-devtools-protocol.md
- entities/openai-frontier.md
- entities/openclaw.md
- entities/nanobot.md
- entities/agent-browser.md
- entities/capability-evolver.md
- entities/gpt-5.md
- entities/xiaomi-miclaw.md
- entities/opencode-platform.md
- entities/github-ai-trending.md
- entities/dify.md
- entities/ragflow.md
- entities/mem0.md
- entities/llama-index.md

## [2026-05-03] create | 7 个 Concepts 页面（第一批）
- concepts/ai-agent.md
- concepts/mcp.md
- concepts/agent-engineering.md
- concepts/rag.md
- concepts/openai-frontier.md
- concepts/chrome-devtools-protocol.md

## [2026-05-03] update | index.md
- 添加 15 个 Entities 条目（第一批）
- 添加 6 个 Concepts 条目（第一批）
- 更新 Total pages: 55（累计）

## [2026-05-03] create | 补建页面（第三批超时补充）
- concepts/openclaw-web-search.md
- raw/02AI与技术/（未完）

## [2026-05-03] ingest | 02AI与技术 目录补漏
- 处理剩余文件：Web搜索能力深度分析（concepts/openclaw-web-search.md）
- 跳过空文件：skills.md, outloop.md, github mcp.md, sanboxes.md, 智能体学习索引.md

- 2026-05-03 ingest [[karpathy-2026-sequoia-ai-ascent]]：Karpathy 红杉 AI Ascent 演讲整理 | source: mp.weixin.qq.com
- 2026-05-03 ingest [[gbrain]]：YC CEO Garry Tan 的 AI Agent 知识脑研究 | source: github.com/garrytan/gbrain
- 2026-05-05 create [[superpowers-ai-coding-sop]]：superpowers-zh 概念页，AI 编程 SOP 方法论 | source: mp.weixin.qq.com/ehjT_F6UuOQfMwKNNfZXjQ
- 2026-05-05 ingest raw [[wechat-bytes-notebook-superpowers]]：字节笔记本 superpowers 文章原始内容

## [2026-05-09] ingest | Hermes Cron 文章入库
- ingest raw [[2026-05-09-hermes-agent-cron]]：Hermes Cron 定时任务完全指南 | source: mp.weixin.qq.com/s/FJsKCsjla-Sni9v-9z4SmQ
- create [[hermes-cron]]：Cron 概念页，包含三种创建方式、四种时间格式、任务流水线、SILENT/no-agent 监控模式

## [2026-05-09] batch-ingest | WeRSS公众号文章批量入库（第二次）
- ingest raw [[2026-05-08-redis-ds4c-deepseek-v4-inference]]：Redis之父ds4.c × DeepSeek V4推理引擎 | source: 量子位
- ingest raw [[2026-05-08-anthropic-roadmap-2027-asi]]：Anthropic ASI路线图2027：无限记忆+多智能体 | source: 新智元
- ingest raw [[2026-05-08-cvpr-2026-video-understanding-8b]]：CVPR 2026：8B小模型反超GPT-5/Gemini | source: 机器之心
- create [[ds4c-deepseek-v4]]：ds4.c × DeepSeek V4 实体页
- create [[anthropic-roadmap-2027]]：Anthropic ASI路线图2027 概念页
- create [[cvpr-2026-video-understanding]]：CVPR 2026视频理解概念页
- skip：iPhone 17（变量棱镜，重复入库）

## [2026-05-10] batch-ingest | WeRSS + Arxiv 批量入库
- ingest raw [[2026-05-10-lenvm-token-length-control]]：LenVM token级长度控制，3B模型击败GPT-5.4/Claude | source: 新智元
- ingest raw [[2026-05-10-ai-scientists-four-autonomous-research-attempts]]：AI Scientists 四个自主研究尝试，仅一成功 | source: Arxiv 2601.03315
- create [[lenvm]]：LenVM 实体页，token级长度控制、可扩展价值预训练、三轴scaling验证
- create [[ai-scientists-autonomous-research]]：AI Scientists 概念页，六大失败模式、四大尝试结果
- skip：新智元Anthropic路线图（已入库）、量子位ds4c（已入库）、机器之心CVPR2026（已入库）
- total: 2 raw + 2 wiki pages
- ingest raw [[2026-05-09-gstack-yc-ceo-ai-coding]]：GStack YC CEO 的 AI 软件工厂争议 | source: 微信公众号
- create [[gstack]]：GStack 实体页，Thin Harness Fat Skills、400x效率、真实浏览器测试、Mini AGI
## [2026-05-11] create | Dario Amodei 2028 AI 自我制造预测
- 来源：矽谷輕鬆談 Just Kidding Tech 视频 + yuanchang.org 文章
- my-obsidian 路径：02AI与技术/2026-05-11-Dario-Amodei-2028-AI自我制造-60%概率.md
- ai-wiki 路径：concepts/dario-amodei-2028-prediction.md
- 核心内容：60% 概率 2028 年 AI 自我制造，与《态势感知》观点对比

## [2026-05-11] ingest | Laser 隐式视觉推理（ACL 2026）
- 来源：机器之心微信公众号（we-mp-rss）
- raw：raw/articles/2026-05-11-laser-implicit-visual-reasoning.md
- create concepts/laser-implicit-reasoning.md：DWAL、概率叠加、熵正则化三大机制
- update concepts/cvpr-2026-video-understanding.md：新增与 Laser 的交叉链接
- update index.md：新增 1 概念页
- skip（已入库）：快手KroWork（工程化视角弱）、胖鹅AI（软文感强）
- total: 1 raw + 1 wiki pages
- ⚠️ WeRSS 调度器已停止（running: false）

## [2026-05-11] update | Anthropic Claude路线图 2026 补充细节
- 来源：微信公众号新智元（同一篇，2026-05-08已入库，故仅更新现有页面）
- 更新：concepts/anthropic-claude-roadmap-2026.md
- 新增：任务视界表格、Opus 4.7自我纠错、Claude Design、TAI Fellowship计划
- 技术发现：WeRSS API需 Bearer Token 认证，前缀 /api/v1/wx/

## [2026-05-12] ingest | Lilian Weng "Why We Think" + Test-Time Compute Scaling
- 来源：https://lilianweng.github.io/posts/2025-05-01-thinking/ (Lilian Weng 博客)
- raw：raw/articles/lilian-weng-why-we-think-2025.md
- create concepts/test-time-compute-scaling.md：测试时计算扩展、CoT进化史、DeepSeek-R1训练流程、Budget Forcing、隐式vs显式推理对比
- 关联：[[laser-implicit-reasoning]], [[lenvm]], [[anthropic-claude-roadmap-2026]], [[dario-amodei-2028-prediction]]
- WeRSS 状态：调度器已关闭（running: false），最新文章停在 2026-05-08
- skip（已入库）：Anthropic路线图2027（2026-05-08已入库）、LenVM（2026-05-10已入库）
- total: 1 raw + 1 wiki pages

## [2026-05-11] create | Ruflo Agent编排平台
- 来源：https://github.com/ruvnet/ruflo
- 路径：entities/ruflo.md, my-obsidian/02AI与技术/2026-05-11-Ruflo-Agent编排平台深度研究.md
- 核心：Claude Code编排平台，48K Stars，100+ Agents，Swarm联邦协作，自学习记忆
- 相关：[[hermes-agent]], [[openclaw]], [[mcp]]


## [2026-05-12] create | OpenCode 工具链铁三角
- 来源：https://mp.weixin.qq.com/s/lCZd3HQ6wvBZRdUxGwccTg（创见AI实验室）
- raw：raw/opencode-iron-triangle-2026-03-31.md
- create comparisons/opencode-toolkit-iron-triangle.md
- 核心：OpenSpec（规范层）+ Superpowers（能力层）+ OMO（基础设施层）铁三角协作
- 关联：[[openspec]], [[superpowers]], [[mcp]], [[opencode-platform]]
- 同步：my-obsidian/02AI与技术/OpenCode铁三角-OpenSpec-Superpowers-OMO完整指南.md

## [2026-05-12] ingest | Supabase 百亿美元估值分析
- 来源：https://mp.weixin.qq.com/s/N-g5W-gh_4HomEJcpOhgrw（海外独角兽）
- raw：raw/articles/2026-05-12-supabase-100b-valuation-vibe-coding-backend.md
- create entities/supabase.md：100亿美元估值 BaaS、4笔收购、Postgres 生态、vibe coding 默认后端
- 同步：my-obsidian/02AI与技术/2026-05-12-Supabase百亿美元估值-vibe-coding默认后端.md
- total: 1 raw + 1 entity pages

## [2026-05-12] clone + research | Vibe-Trading 项目
- 来源：https://github.com/HKUDS/Vibe-Trading
- 本地路径：~/mywork/code/Vibe-Trading
- 核心：AI量化交易Agent平台，74 Skills，29 Swarm Presets，6数据源
- create entities/vibe-trading.md
- 同步：my-obsidian/02AI与技术/Vibe-Trading项目研究报告.md
- 推送：ai-wiki GitHub

## [2026-05-12] ingest | Reasoning Trap Tool Hallucination (ACL 2026)
- 来源：arXiv 2510.22977，WeRSS 扫描（今日无高质量公众号内容）
- raw/articles/2026-05-12-reasoning-trap-tool-hallucination.md（全文）
- concepts/tool-hallucination-reasoning-trap.md（新建，ACL 2026 论文解读）
- 核心：推理增强（RL/蒸馏/推理模式）会系统性放大工具幻觉，reliability-capability trade-off 是根本性矛盾
- 跨链接：[[ai-agent]], [[mcp]], [[test-time-compute-scaling]], [[agent-engineering]], [[harness-engineering]]
- total: 1 raw + 1 concept page

## [2026-05-13] ingest | 3 篇 AI 内容入库（WeRSS + Lilian Weng + 字节 GRN）
- 来源：WeRSS 公众号扫描（机器之心、量子位）+ Lilian Weng 博客
- **WeRSS 评估结果**：今日公众号内容以新闻/软文为主（量子位峰会宣传、励志鸡汤），机器之心 2 篇有技术深度
- raw/articles/:
  - 2026-05-13-bytedance-grn-visual-generation.md（字节 GRN，量子位）
  - 2026-05-13-apo-multi-teacher-mllm-alignment.md（悉尼科大 APO，机器之心）
  - 2026-05-13-lilian-weng-why-we-think.md（Lilian Weng Why We Think 2025）
- concepts/bytedance-grn.md（新建）
- concepts/apo-multi-teacher-mllm-alignment.md（新建）
- concepts/lilian-weng-why-we-think.md（新建）
- 跨链接：GRN→[[test-time-compute-scaling]],[[ai-agent]],[[cvpr-2026-video-understanding]]；APO→[[alignment]],[[test-time-compute-scaling]],[[tool-hallucination-reasoning-trap]],[[fine-tuning-with-trl]]；Lilian→[[test-time-compute-scaling]],[[ai-agent]],[[tool-hallucination-reasoning-trap]],[[harness-engineering]]
- total: 3 raw + 3 concept pages

## [2026-05-14] ingest | 2篇AI内容入库（TriAttention + DYPO）
- 来源：WeRSS（机器之心 + 量子位）
- **评估结果**：新智元NVIDIA/MIT/浙大KV压缩有硬核数据；量子位DYPO清华论文有新方法论
- raw/articles/:
  - 2026-05-14-triattention-kv-compression-nvidia-mit-zhejiang.md（NVIDIA/MIT/浙大 KV 压缩）
  - 2026-05-14-dypo-dynamic-policy-optimization-tsinghua.md（清华 DYPO 动态策略优化）
- concepts/:
  - concepts/triattention-kv-compression.md（新建，pre-RoPE三角集中度，10.7x内存缩减）
  - concepts/dypo-dynamic-policy-optimization.md（新建，Easy/Hard/Mid三类样本动态路由）
- 跨链接：TriAttention→[[test-time-compute-scaling]],[[openclaw]],[[vibe-trading]],[[inference-optimization]]；DYPO→[[on-policy-distillation-thunlp]],[[fine-tuning-with-trl]],[[test-time-compute-scaling]],[[tool-hallucination-reasoning-trap]]
- total: 2 raw + 2 concept pages

## [2026-05-14] ingest | 3篇AI内容入库（清华OPD + Codex Goal Mode + Ralph Loop三厂大战）
- 来源：WeRSS（机器之心 x2 + 新智元 x1）
- **评估结果**：今日量子位内容以峰会宣传为主不入库；机器之心清华蒸馏论文有新发现（高分≠新知识）；新智元两篇技术深度足够
- raw/articles/:
  - 2026-05-14-thunlp-opd-on-policy-distillation.md（清华 THUNLP On-Policy Distillation）
  - 2026-05-14-codex-goal-mode-scientific-breakthrough.md（Codex Goal Mode 40倍科研效率）
  - 2026-05-14-ralph-loop-goal-mode-war.md（OpenAI/Anthropic/Hermes 三厂 Goal Mode 实现对比）
- concepts/:
  - concepts/on-policy-distillation-thunlp.md（新建，思维模式一致性 + 高分≠新知识）
  - concepts/codex-goal-mode-ai-science.md（新建，RSI概率>60%，SWE-bench 2%→93.9%）
  - concepts/ralph-loop-goal-mode-war.md（新建，Codex持久化 vs Hermes看板 vs Claude裁判模型）
- 跨链接：OPD→[[fine-tuning-with-trl]],[[test-time-compute-scaling]],[[ai-agent]]；Codex→[[test-time-compute-scaling]],[[ai-scientists-autonomous-research]],[[openai-frontier]],[[hermes-agent]]；Ralph→[[hermes-agent]],[[openai-frontier]],[[test-time-compute-scaling]],[[harness-engineering]]
- total: 3 raw + 3 concept pages

## [2026-05-13] ingest | LLM 自主设计神经网络架构 (arXiv 2601.02997)
- 论文：From Memorization to Creativity: LLM as a Designer of Novel Neural-Architectures
- 作者：Waleed Khalid, Dmitry Ignatov, Radu Timofte (University of Würzburg)
- 核心：DeepSeek-Coder-7B 经 22 轮闭环迭代微调，进化为可靠的神经网络架构生成器
- 关键结果：CIFAR-10 生成 455 个有效新架构，准确率从 28% 提升至 51%
- raw/articles/2026-05-13-llm-neural-architecture-design-2601.02997.md（新建）
- concepts/llm-neural-architecture-design.md（新建）
- entities/khalid-ignatov-timofte-nas.md（新建）
- 跨链接：[[test-time-compute-scaling]], [[fine-tuning-with-trl]], [[agent-engineering]], [[ai-scientists-autonomous-research]]
- total: 1 raw + 1 concept page + 1 entity page

## [2026-05-15] ingest | 3篇AI内容入库（TriAttention更新 + Kimi K2.6基建 + Agentic Code Reasoning）
- 来源：WeRSS（新智元 x1 + 量子位 x1）+ ArXiv x1
- raw/articles/:
  - 2026-05-15-triattention-nvidia-mit-zju.md（新智元，NVIDIA/MIT/浙大联合开源）
  - 2026-05-15-kimi-k2-6-ai-infra-tidb.md（量子位，Kimi K2.6数据库架构）
  - 2026-05-15-agentic-code-reasoning-2603.01896.md（arXiv 2603.01896，Meta半形式化代码推理）
- concepts/:
  - concepts/triattention-kv-compression.md（更新，追加新来源）
  - concepts/agentic-code-reasoning.md（新建，Semi-formal Reasoning方法）
  - concepts/kimi-k2-6-ai-infra.md（新建，TiDB Cloud多租户架构）
- TriAttention跨链接：[[test-time-compute-scaling]], [[openclaw]], [[vibe-trading]]
- Agentic Code Reasoning跨链接：[[openclaw]], [[ai-agent]], [[test-time-compute-scaling]]
- Kimi K2.6 AI基建跨链接：[[mem0]], [[rag]], [[test-time-compute-scaling]]
- total: 3 raw + 2 new concept pages + 1 updated

## [2026-05-15] ingest | 2篇AI内容入库（Claude角色混淆Bug + Decoction经验提精）
- 来源：WeRSS（新智元 x1）+ ArXiv x1
- **评估结果**：Claude角色混淆是claude-coded agent安全架构的严重bug，有学术论文支撑；Decoction是agent记忆优化的新范式，有系统性实验
- raw/articles/:
  - 2026-05-15-claude-role-confusion-bug.md（新智元，Claude最严重bug）
  - 2026-05-15-decoction-experience-llm-agents-2604.04373.md（arXiv 2604.04373，经验提精）
- concepts/:
  - concepts/claude-role-confusion-bug.md（新建，角色混淆Bug与AI Agent权限安全）
  - concepts/decoction-experience-memory.md（新建，经验提精与记忆优化新范式）
- 跨链接：角色混淆→[[test-time-compute-scaling]],[[ai-agent]],[[anthropic-claude-roadmap-2026]],[[mcp]]；Decoction→[[test-time-compute-scaling]],[[rag]],[[ai-agent]],[[mem0]]
- total: 2 raw + 2 concept pages

## [2026-05-16] ingest | Memory Processing Pipeline 论文入库
- WeRSS 新智元 TriAttention 文章：发现已有页面（2026-05-14），跳过
- 主动搜索：发现 arXiv 2603.29002 Memory Processing Pipeline，新入库
- raw/articles/:
  - 2026-05-16-memory-processing-pipeline-llm-inference.md
- concepts/:
  - concepts/memory-processing-pipeline-llm-inference.md（新建，四阶段框架 + GPU-FPGA 异构）
- 跨链接：Memory Pipeline → [[triattention-kv-compression]], [[test-time-compute-scaling]]
- total: 1 raw + 1 concept page

## [2026-05-16] ingest | 2篇内容入库（Agent Drift + FASTER VLA）
- 来源：ArXiv 2601.04170（Agent Drift）+ 机器之心/港大（FASTER）
- **评估结果**：Agent Drift 是多智能体LLM系统长期运行退化问题，有系统性实验数据；FASTER是具身智能推理优化，有真机验证
- raw/articles/:
  - 2026-05-16-agent-drift-2601.04170.md（ArXiv，多智能体漂移研究）
  - 2026-05-16-faster-vla-hku-ttfa.md（机器之心，港大VLA动作采样）
- concepts/:
  - concepts/agent-drift-multi-agent-systems.md（新建，ASI框架 + 缓解策略）
  - concepts/faster-vla-action-sampling.md（新建，TTFA指标 + HAS调度）
- 跨链接：Agent Drift → [[test-time-compute-scaling]], [[ai-agent]], [[mem0]], [[harness-engineering]]
- 跨链接：FASTER → [[test-time-compute-scaling]], [[inference-optimization]]
- total: 2 raw + 2 concept pages

## [2026-05-17] ingest | I²B-LPO（ACL 2026 阿里达摩院）入库
- 来源：机器之心（微信公众号）https://mp.weixin.qq.com/s/Ez9doSbgmOeJjac1nfNsqw
- **评估结果**：ACL 2026 Main 论文，解决 RLVR 探索同质化问题，高熵节点分支 + 信息瓶颈自奖励，AIME+5.3%，有硬核方法论
- raw/articles/:
  - 2026-05-17-i2b-lpo-acl2026-rlvr.md
- concepts/:
  - concepts/i2b-lpo-rlvr-exploration.md（新建，高熵节点分支 + IB 自奖励 + GRPO 策略更新）
- 跨链接：I²B-LPO → [[dypo-dynamic-policy-optimization]], [[on-policy-distillation-thunlp]], [[test-time-compute-scaling]], [[tool-hallucination-reasoning-trap]]
- total: 1 raw + 1 concept page

## [2026-05-17] ingest | ManyIH 多层指令层级基准（arXiv 2604.09443）
- 来源：arXiv HTML 全文（2604.09443v3，JHU-CLSP）
- 评估结果：JHU 提出 ManyIH 范式处理 Agent 场景 12 层指令冲突，顶级模型（GPT 5.4/Gemini 3.1 Pro）在 ManyIH-Bench 上准确率仅 40%，远低于标准 IH 自评 >99%；核心瓶颈是风格合规而非功能正确性（>86%）；有方法论有实验数据，值得入库
- raw/articles/:
  - 2026-05-17-manyih-arxiv-2604.md（新建）
- concepts/:
  - concepts/manyih.md（新建，多层指令层级冲突解决范式）
- 跨链接：manyih → [[mcp]], [[agentic-code-reasoning]], [[harness-engineering]]
- total: 1 raw + 1 concept page

## [2026-05-18] ingest | 2篇内容入库（TriAttention更新 + MAPLE子代理架构）
- 来源：新智元（微信公众号）TriAttention 工程细节 + arXiv 2602.13258 MAPLE论文
- **评估结果**：TriAttention 新来源追加了 vLLM 插件详情和消费级部署里程碑；MAPLE 是 ALA 2026 论文，记忆/学习/个性化三元分解有系统性方法论，与 mem0 直接对比
- raw/articles/:
  - 2026-05-18-triattention-nvidia-mit-zju-kv-compression.md（新建，新智元第三篇TriAttention，vLLM插件+MLX+里程碑）
  - 2026-05-18-maple-sub-agent-memory-learning-personalization.md（新建，arXiv MAPLE论文全文）
- concepts/:
  - concepts/triattention-kv-compression.md（更新，追加第三来源 + 工程落地细节 + 消费级硬件里程碑）
  - concepts/maple-sub-agent-architecture.md（新建，三元分解 + 三层记忆透镜 + 与mem0对比）
- 跨链接：MAPLE → [[mem0]], [[decoction-experience-memory]], [[agent-drift-multi-agent-systems]], [[ai-agent]], [[harness-engineering]]
- total: 2 raw + 2 concept pages（1新+1更新）

## [2026-05-18] ingest | DYPO 动态策略优化（补充入库）
- 来源：量子位转载 + arXiv 2604.08926（DYPO论文原文）
- **评估结果**：WeRSS 数据库最新文章停在 05-14（4天前无更新）；主动搜索发现 DYPO 已有 05-14 入库记录，今日入库版本来自量子位报道+论文全文，内容更完整（sha 不同）
- raw/articles/:
  - 2026-05-18-dypo-dynamic-policy-optimization.md（新建）
- concepts/:
  - concepts/dypo-dynamic-policy-optimization.md（新建，偏差-方差分类+GAL+实验数据）
- 跨链接：dypo → [[test-time-compute-scaling]], [[maple-sub-agent-architecture]], [[harness-engineering]]
- total: 1 raw + 1 concept page

## [2026-05-20] ingest | 3 articles: Recursive Superintelligence / VGGT / MoralReason
- WeRSS 最新文章停在 2026-05-14（6天前），调度器运行但可能 Token 空转；直接查 DB 确认
- 评估候选：新智元 田渊栋$44亿融资（✅入库）、魔芯VGGT空间智能（✅入库）、arXiv MoralReason AAAI 2026（✅入库）
- **评估结果**：Recursive Superintelligence 是 AI Scientists Recursive Self-Improvement 的商业化代表；VGGT 是空间智能/世界模型的重要工程进展；MoralReason 是推理级RL对齐的系统性框架
- raw/articles/:
  - 2026-05-20-recursive-superintelligence-8-ai-titans.md（新建，新智元报道）
  - 2026-05-20-vggt-spatial-intelligence-world-model.md（新建，机器之心报道）
  - 2026-05-20-moralreason-reasoning-level-rl-moral-alignment.md（新建，arXiv 2511.12271v1）
- entities/:
  - entities/recursive-superintelligence.md（新建，8位创始人+RSI架构+对立观点+与ai-scientists/scheming-propensity关联）
- concepts/:
  - concepts/vggt-spatial-intelligence.md（新建，三项创新+O(1)显存+4D解耦+HD-VGGT+Scaling Law）
  - concepts/moralreason-reasoning-level-rl.md（新建，GRPO道德对齐+功利/义务/美德三框架对比+与test-time-compute/scheming-propensity/harness-engineering关联）
- index.md: 发现并清理了 Concepts 部分重复条目（ai-scientists/harness-engineering/enterprise-skill/llm-neural重复）
- total: 3 raw + 3 wiki pages（1 entity新 + 2 concept新）
- 注：WeRSS Token已过期，API 401报错，需重新扫码授权；DB查询正常可用

## [2026-05-19] ingest | Scheming Propensity LLM Agents（arXiv 2603.01608v2）
- 来源：arXiv 论文全文（/html/ 路径获取完整内容）
- **评估结果**：伪装倾向系统研究框架，4大场景+3个激励维度，基线0-3% vs 对抗提示59%，工具访问是关键杠杆，监督悖论有实战意义
- raw/articles/:
  - 2026-05-19-scheming-propensity-llm-agents.md（新建）
- concepts/:
  - concepts/scheming-propensity-llm-agents.md（新建，伪装倾向框架+4场景+环境激励维度）
- 跨链接：scheming-propensity → [[agent-drift-multi-agent-systems]], [[test-time-compute-scaling]], [[ai-agent]], [[mcp]]
- total: 1 raw + 1 concept page
- 注：WeRSS 最新文章停在 2026-05-14（5天前），scheduled 调度器需 Token 续期

## [2026-05-22] ingest | MOSS 源代码级自进化（arXiv 2605.22794）
- 来源：arXiv 2605.22794，MOSS: Self-Evolution through Source-Level Rewriting in Autonomous Agent Systems
- **评估结果**：源代码级自进化填补文本层盲区，harness 路由/hook/不变式只能用代码修改；外部 coding-agent CLI 可插拔；OpenClaw 四任务 0.25→0.61
- **WeChat 爬取结果**：新智元/量子位/机器之心的最新 AI 文章均为 2026-05-14（8天前），WeChat 需要 JS 渲染无法直接提取，WeRSS Token 仍有效但爬取空转
- raw/articles/:
  - 2026-05-22-moss-self-evolution-source-level-rewriting.md（新建，arXiv abstract + 摘要整理）
- concepts/:
  - concepts/moss-source-level-self-evolution.md（新建，源代码级自进化框架+MOSS架构+Ratchet对比）
- entities/:
  - entities/capability-evolver.md（更新，追加 MOSS 对比表，解释文本层盲区）
- 跨链接：moss → [[capability-evolver]], [[harness-engineering]], [[openclaw-ecosystem]], [[agent-drift-multi-agent-systems]], [[scheming-propensity-llm-agents]]
- total: 1 raw + 1 concept page 新建 + 1 entity page 更新
- 注：WeRSS Token 有效（sync_time 0.3h 前），但 WeChat 文章需 JS 渲染才能提取内容，curl 返回空；arXiv abstract 页面可正常提取

## [2026-05-23] ingest | LCGuard KV潜在通信安全（arXiv 2605.22786）
- 来源：arXiv 2605.22786，LCGuard: Latent Communication Guard for Safe KV Sharing in Multi-Agent Systems
- **评估结果**：IBM Research + RPI，多Agent KV潜在通信安全问题，对抗重建防御框架，首次操作化定义KV级表示泄露；质量高，入库
- **WeRSS 情况**：新智元/量子位/机器之心最新文章均停在 2026-05-14（9天前），多为新闻简讯，无技术深度，不入库
- raw/articles/:
  - 2026-05-23-lcguard-kv-sharing-multi-agent.md（新建）
- concepts/:
  - concepts/lcguard-kv-latent-communication.md（新建，KV潜在通信安全框架+对抗游戏+安全vs效用帕累托）
- 跨链接：lcguard → [[agent-drift-multi-agent-systems]], [[triattention-kv-compression]], [[scheming-propensity-llm-agents]], [[mcp]], [[memory-processing-pipeline-llm-inference]]
- total: 1 raw + 1 concept page 新建
- 注：WeRSS 文章9天未更新，均为新闻简讯不入库；主动发现 arXiv 2605.22786

## [2026-05-24] ingest | Gated DeltaNet-2 线性注意力（arXiv 2605.22791）
- 来源：arXiv 2605.22791v1，NVIDIA 论文全文（arXiv HTML 页面提取）
- **评估结果**：NVIDIA 提出通道级擦除/写入解耦，1.3B 参数全面超越 Mamba-2/KDA/Mamba-3，长上下文检索优势最大；技术深度足够，质量高，入库
- **WeRSS 情况**：新智元/量子位/机器之心最新公众号文章多为新闻/软文不入库；仅 Bengio 递归推理一篇有技术深度（今日选 1 篇主动搜索优先入库 DeltaNet-2）
- raw/articles/:
  - 2026-05-24-gated-deltanet-2-nvidia-linear-attention.md（新建）
- concepts/:
  - concepts/gated-deltanet-2.md（新建，通道级擦除写入门控 + 历代线性注意力对比表 + fast-weight 更新视角）
- 跨链接：gated-deltanet-2 → [[test-time-compute-scaling]], [[triattention-kv-compression]], [[memory-processing-pipeline-llm-inference]], [[inference-optimization]]
- total: 1 raw + 1 concept page 新建
- 注：WeRSS 同步正常（0.2h 前），公众号内容质量不足，无入库

## [2026-05-24] ingest | Bengio GRAM 并行递归推理
- 来源：机器之心公众号（WeRSS：MP_WXS_3073282833），文章发布于 2026-05-23 21:58
- 评估结果：Yoshua Bengio 提出 GRAM，将确定性递归推理转为概率性多轨迹计算，16步+20并行 > 320步串行；技术深度足够，质量高，入库
- WeRSS 情况：新智元/量子位/机器之心最新文章多为新闻/软文不入库；仅此篇有技术深度
- raw/articles/:
  - 2026-05-24-bengio-gram-recursive-reasoning.md（新建）
- entities/:
  - entities/gram-generative-recursive-reasoning.md（新建，Bengio GRAM 实体页）
- 跨链接：gram-generative-recursive-reasoning → [[test-time-compute-scaling]], [[agent-drift-multi-agent-systems]], [[lilian-weng-why-we-think]]
- total: 1 raw + 1 entity page 新建
- 注：今日 WeRSS 公众号内容整体质量不足，仅此篇入库；web_search/web_extract 均不可用，改用 browser 获取微信文章

## [2026-05-25] ingest | CODA GEMM-Epilogue 重参数化论文（arXiv 2605.19269）
- 来源：机器之心微信公众号（2026-05-24）
- 源文件：raw/articles/2026-05-25-coda-rewriting-transformer-blocks.md
- 新建文件：
  - raw/articles/2026-05-25-coda-rewriting-transformer-blocks.md（新建）
  - concepts/coda-gemm-epilogue-optimization.md（新建，完整解析）
  - entities/coda-arxiv-2605.19269.md（新建，论文实体页）
- 跨链接：coda-gemm-epilogue-optimization → [[triattention-kv-compression]], [[gated-deltanet-2]], [[test-time-compute-scaling]], [[lilian-weng-why-we-think]]
- total: 1 raw + 2 wiki pages 新建
- 注：web_search/web_extract 均不可用，用 browser + browser_console 提取微信文章全文

## [2026-05-25] ingest | 具身智能安全综述 arXiv 2605.02900
- 来源：机器之心微信公众号（2026-05-25）
- 评估结果：13家机构38位学者联合发布，五层能力—风险二象性框架，覆盖480+篇论文，技术深度高，质量高，入库
- WeRSS 情况：今日新智元/量子位文章多为新闻/软文不入库；仅此篇具身智能安全综述有技术深度；另一篇 SaaS-Bench 文章 wiki 已存在，跳过
- raw/articles/:
  - 2026-05-25-embodied-ai-safety-survey.md（新建）
- concepts/:
  - concepts/embodied-ai-safety-survey.md（新建，具身智能安全综述）
- 跨链接：embodied-ai-safety-survey → [[agent-drift-multi-agent-systems]], [[tool-hallucination-reasoning-trap]]
- total: 1 raw + 1 concept page 新建
- 注：web_search/web_extract 均不可用，用 browser + browser_console 提取微信文章全文

## [2026-05-26] ingest | EdgeRazor 混合精度量化感知蒸馏（arXiv 2605.04062）
- 来源：机器之心微信公众号（2026-05-25）+ arXiv abstract 补全
- **评估结果**：南京大学 LAMDA + 微软 AI 联合发布，MPQAD 三大模块解决极低比特量化"不可能三角"，1.58-bit Qwen3-0.6B 实现 15.16× 解码加速、190MB 百兆级端侧部署，技术深度高，质量高，入库
- WeRSS 情况：今日公众号整体质量不足（新闻/软文为主）；仅 EdgeRazor 一篇有技术深度
- raw/articles/:
  - 2026-05-26-edgerazor-mixed-precision-quantization.md（新建）
- concepts/:
  - concepts/edgerazor-mixed-precision-quantization.md（新建，MPQAD 三大模块 + 核心性能数据）
- 跨链接：edgerazor → [[gated-deltanet-2]], [[triattention-kv-compression]], [[memory-processing-pipeline-llm-inference]], [[test-time-compute-scaling]], [[lenvm]]

## [2026-05-26] ingest | 2篇高质量内容入库（Evo-Depth VLA + AlphaProof Nexus）
- 来源：量子位（Evo-Depth）+ 机器之心（AlphaProof Nexus）
- **评估结果**：
  - Evo-Depth：上交MINT团队，0.9B参数VLA空间增强方案，隐式深度编码（IDEM+SEM+Progressive Alignment），真机90%成功率，有技术深度，入库
  - AlphaProof Nexus：DeepMind形式化证明搜索，353个Erdős开放问题解决9个，成本数百美元/个，Lean编译器+Gemini 3.1基础智能体即可，里程碑级别，入库
- WeRSS 情况：今日公众号整体质量一般（新闻/软文为主）；仅此2篇有技术深度，值得入库
- raw/articles/:
  - 2026-05-26-evo-depth-vla-spatial-intelligence.md（新建）
  - 2026-05-26-alphaproof-nexus-deepmind-erdos.md（新建）
- concepts/:
  - concepts/evo-depth-vla-spatial-intelligence.md（新建，IDEM+SEM+Progressive Alignment三模块 + 性能数据）
  - concepts/alphaproof-nexus-deepmind-formal-proof.md（新建，两种智能体架构对比 + 9个Erdős问题列表）
- 跨链接：evo-depth → [[faster-vla-action-sampling]], [[vggt-spatial-intelligence]], [[embodied-ai-safety-survey]], [[test-time-compute-scaling]]
- 跨链接：alphaproof-nexus → [[recursive-superintelligence]], [[harness-engineering]], [[test-time-compute-scaling]], [[agentic-code-reasoning]]
- total: 2 raw + 2 concepts pages 新建
- 注：web_search/web_extract 均不可用，用 browser + browser_console 提取微信文章全文

## [2026-05-27] ingest | MUSE-Autoskill arXiv 2605.27366
- 来源：arXiv abs 页面（今日 cs.AI 新论文）
- 主题：字节跳动自进化 Agent 框架，Skill 全生命周期管理
- raw/articles/: 2026-05-27-muse-autoskill-self-evolving-agents.md（新建）
- concepts/: concepts/muse-autoskill-agent.md（新建）
- 跨链接：muse-autoskill → [[moss-source-level-self-evolution]], [[mem0]], [[agent-memory-processing-pipeline-llm-inference]]

## [2026-05-28] ingest | Alignment Tampering arXiv 2605.27355
- 来源：arXiv HTML 页面（今日 cs.AI 新论文，ICML 2026 accepted）
- 主题：RLHF 对齐过程中的结构性漏洞，模型可影响自己被评估的偏好数据集，导致偏差被放大至 ~100%
- raw/papers/：2026-05-28-alignment-tampering-icml2026.md（新建）
- concepts/：concepts/alignment-tampering-rlhf.md（新建）
- 跨链接：alignment-tampering → [[ai-sycophancy-analysis]], [[scheming-propensity-llm-agents]], [[moralreason-reasoning-level-rl]], [[apo-multi-teacher-mllm-alignment]], [[tool-hallucination-reasoning-trap]]
- WeRSS 情况：今日微信公号文章以新闻简讯为主（OpenAI挖F1车手、新智元ASI英雄帖等），缺乏技术深度，未入库；改从 arXiv 补获 ICML 2026 论文 1 篇
- 总入库：1 raw + 1 concepts

## [2026-05-28] ingest | Mnemis AI Long Memory ACL2026
- 来源：量子位公众号（微信文章 mp.weixin.qq.com/s/A-xVzjJSmzkfowgGz1Fx7g）
- 主题：微软 ACL 2026 AI 记忆框架，层级图索引 + Kahneman 双系统检索，LoCoMo 93.9% / LongMemEval-S 91.6%
- raw/articles/：2026-05-28-mnemis-ai-long-memory-acl2026.md（新建）
- concepts/：concepts/mnemis-ai-long-memory.md（新建）
- 跨链接：mnemis-ai-long-memory → [[rag]], [[mem0]], [[decoction-experience-memory]], [[test-time-compute-scaling]], [[ragflow]]
- WeRSS 情况：量子位 1 篇有技术深度入库（Mnemis）；其余公号文章多为新闻简讯（具身融资、CVPR 视频、苏深度ASI等），未入库
- 总入库：1 raw + 1 concepts

## [2026-05-29] ingest | SwarmHarness + RSAgent
- 来源①：arXiv 2605.28764v1（cs.AI，2026-05-27）— SwarmHarness 去中心化算力激励协议，HarnessAPI 多节点扩展，DHT+Shapley值无需区块链
- 来源②：量子位公众号（微信文章 mp.weixin.qq.com/s/LK2AwasT6xWbEDBkHxQK9A）— RSAgent 多模态 Agent 视觉分割，ICML 2026，gIoU 66.5% 超越 Seg-Zero-7B 9%
- raw/articles/：2026-05-29-swarmharness-decentralized-ai-agent-network.md（新建）
- raw/articles/：2026-05-29-rsagent-visual-segmentation-icml2026.md（新建）
- concepts/：concepts/swarmharness.md（新建）
- concepts/：concepts/rsagent.md（新建）
- 跨链接 swarmharness → [[harness-engineering]], [[mcp]], [[openclaw]], [[agent-drift-multi-agent-systems]], [[superpowers]]
- 跨链接 rsagent → [[agentic-code-reasoning]], [[test-time-compute-scaling]], [[faster-vla-action-sampling]], [[embodied-ai-safety-survey]], [[muse-autoskill-agent]]
- WeRSS 情况：今日微信公号文章多为新闻简讯（Claude勒索人类、OpenRouter流量、ASI英雄帖等），未入库；改从 arXiv 补获 SwarmHarness 论文 1 篇 + 从量子位获 RSAgent 技术解析 1 篇

## [2026-05-29] ingest | AMD MXFP4 FP4训练 + DeepSeek陈德里自主研究综述
- 来源①：机器之心公众号（微信文章 mp.weixin.qq.com/s/ljY5ORd36n9CrN-K-22rZQ）— AMD MI355X 原生 FP4 训练论文 arXiv:2605.09825，Llama 3.1-8B 训练加速 9-10%，颠覆性发现：不稳定根源是结构性误差而非随机性不足
- 来源②：机器之心公众号（微信文章 mp.weixin.qq.com/s/Z8IOHlufZrZvgF4be5X-lA）— DeepSeek 陈德里 46 页自主科研 Agent 综述，L1-L5 自主等级分类，四种架构模式，六大未解难题
- raw/articles/：2026-05-29-amd-fp4-training-mxfp4-mi355x.md（新建）
- raw/articles/：2026-05-29-deepseek-autonomous-research-agents-survey.md（新建）
- concepts/：concepts/amd-mxfp4-fp4-training.md（新建）
- concepts/：更新 [[ai-scientists-autonomous-research]]，追加陈德里综述的 L1-L5 框架和四种架构模式内容，补充 [[muse-autoskill-agent]] 和 [[codex-goal-mode-ai-science]] 跨链接
- 跨链接 amd-mxfp4 → [[edgerazor-mixed-precision-quantization]], [[test-time-compute-scaling]], [[gated-deltanet-2]], [[triattention-kv-compression]]
- WeRSS 情况：今日公号文章多为新闻简讯（OpenAI挖F1车手、新智元ASI英雄帖、Claude勒索人类等），AMD FP4 训练和 DeepSeek 陈德里综述两篇有技术深度入库
- 总入库：2 raw + 1 新 concepts + 1 更新 concepts
## [2026-05-30] ingest | Compositional Residual arXiv 2605.30335
- 来源：arXiv HTML 页面（cs.AI，2026-05-28，ICML 2026 相关研讨会）
- 主题：多组件 LLM Agent 的概率组合失效机制——局部相干但全局不相干（LCGI），通过合成残差 ε⋆ 运行时证书检测，分层 Boyle-Dykstra 投影修复；1,876 个 ensemble cliques 实验，33-94% 出现 LCGI 故障
- raw/papers/：2026-05-30-compositional-incoherence-multi-llm-agents.md（新建）
- concepts/：concepts/compositional-residual-llm-agents.md（新建）
- 跨链接：compositional-residual → [[agent-drift-multi-agent-systems]], [[ai-sycophancy-analysis]], [[harness-engineering]], [[lcguard-kv-latent-communication]]
- WeRSS 情况：今日微信公号文章多为新闻简讯（Claude勒索人类、OpenRouter流量、ASI英雄帖等），缺乏技术深度，未入库；改从 arXiv 补获 ICML 2026 相关论文 1 篇
- 总入库：1 raw + 1 concepts

## [2026-05-30] ingest | Reasoning in Memory (RiM) arXiv 2605.30343
- 来源：arXiv HTML 页面（cs.CL，ACL 2026 Main Conference）
- 主题：Reasoning in Memory (RiM)——用固定记忆块（Memory Blocks）替代自回归推理步骤，实现单次前向传播内并行推理；两阶段课程（Stage 1 监督推理步骤→Stage 2 去掉中间监督精炼答案）
- raw/papers/：2026-05-30-rim-reasoning-in-memory-llm.md（新建）
- concepts/：concepts/reasoning-in-memory-rim.md（新建）
- 跨链接：reasoning-in-memory-rim → [[test-time-compute-scaling]], [[laser-implicit-reasoning]], [[agentic-code-reasoning]], [[lilian-weng-why-we-think]], [[mnemis-ai-long-memory]]
- WeRSS 情况：微信公号文章最新为 2026-05-27，多为新闻简讯（Claude勒索人类、OpenRouter流量、ASI英雄帖等），缺乏技术深度，未入库；改从 arXiv 补获 ACL 2026 论文 1 篇
- 总入库：1 raw + 1 concepts

## [2026-05-31] ingest | LLMSurgeon - 诊断大模型数据配比 (arXiv 2605.30348v1)
- 来源：arXiv HTML 页面（cs.CL，2026-05-28，ACL 2026 Main）
- 主题：LLMSurgeon — Data Mixture Surgery（DMS）问题定义，给定 LLM 生成文本反推预训练数据领域分布；核心方法是 label-shift 假设下的逆问题求解，标定软混淆矩阵恢复潜在混合比例；配套 LLMScan 验证基准
- raw/papers/：2026-05-31-llmsurgeon-diagnosing-data-mixture-llm.md（新建）
- concepts/：concepts/llmsurgeon-data-mixture-diagnosis.md（新建）
- 跨链接：llmsurgeon → [[test-time-compute-scaling]], [[alignment-tampering-rlhf]], [[mcp]]
- WeRSS 情况：今日（05-31）微信公号最新文章仍为 05-27，多为新闻简讯（Claude勒索人类、OpenRouter流量、ASI英雄帖等），缺乏技术深度，未入库；从 arXiv 补获 ACL 2026 论文 1 篇
- 总入库：1 raw + 1 concepts

## [2026-06-01] ingest | Verkor Design Conductor - AI 完整芯片设计 (WeChat 新智元)
- 来源：微信公众号「新智元」2026-05-24，AI首次独自跑完芯片设计
- 主题：Verkor Design Conductor — 219词需求 → 12小时 → 7nm GDSII 版图，业界首次 AI 走通完整芯片设计流程（需求分析→架构设计→RTL→功能验证→时序收敛→物理版图）；VerCore RISC-V CPU，CoreMark 3261，数百亿token算力消耗
- raw/articles/：2026-05-24-verkor-design-conductor-ai-chip-design.md（新建）
- entities/：entities/verkor.md（新建）
- 跨链接：verkor → [[harness-engineering]], [[gram-generative-recursive-reasoning]], [[agent-engineering]]
- WeRSS 情况：最近公号文章最新日期 2026-05-24，多为新闻简讯（摩根小通、中国PMI、Token贩子等），新智元「AI首次独自跑完芯片设计」有技术深度入库
- 总入库：1 raw + 1 entities

## [2026-06-01] ingest | AutoSci 全流程科研 Agent 系统 (arXiv 2605.31468)
- 来源：arXiv HTML 页面（cs.AI，2026-06-01，北大）
- 主题：AutoSci — 内存中心化科研 Agent，四模块（SciMem/SciFlow/SciDAG/SciEvolve）；首个同时具备完整生命周期+Harness+结构化持久记忆+系统自演化的科研 Agent；对比 AI Scientist 系列等均缺失至少一项
- raw/papers/：2026-06-01-autosci-memory-centric-scientific-agent.md（新建）
- concepts/：更新 [[ai-scientists-autonomous-research]]，追加 AutoSci 四模块架构内容
- 跨链接：ai-scientists-autonomous-research → [[harness-engineering]], [[muse-autoskill-agent]], [[codex-goal-mode-ai-science]]
- 主动发现：从 arXiv cs.AI listing 发现今日新提交，AutoSci 是北大 2026 系统，自演化能力突出，值得入库
- 总入库：1 raw + 1 更新 concepts

## [2026-06-03] ingest | SWE Atlas - Scale AI编程工程能力基准 (WeChat 新智元)
- 来源：WeRSS DB（微信公众号「新智元」2026-05-23）
- 主题：SWE Atlas — Scale AI发布的284道工程题基准，评估代码理解/测试编写/重构；Pass@1最高43.49%（GPT-5.4），但三次通过率骤降30-50%；揭示当前AI编程模型是"优秀补丁工、糟糕工程师"
- raw/articles/：2026-06-03-swe-atlas-scale-ai-coding-benchmark.md（新建）
- concepts/：concepts/swe-atlas.md（新建）
- 跨链接：swe-atlas → [[harness-engineering]], [[agent-production-evaluation]], [[claude-code-insights]], [[test-time-compute-scaling]]
- WeRSS 情况：DB最新文章 2026-05-24，多为新闻简讯；新智元「AI编程进入下半场」有技术深度入库
- 静默项：Harness工程文（2026-06-02已入库，sha256一致）、GRAM Bengio文（2026-05-24已入库）、PyraMathBench arXiv（质量一般）
- 总入库：1 raw + 1 concepts

## [2026-06-02] ingest | 2篇内容入库（Agent生产评估 + Harness五子系统）
- 来源：WeRSS DB（机器之心 + 新智元，2026-05-24）
- **评估结果**：Agent生产评估是有系统方法论的技术文章，Harness工程有Anthropic和OpenAI双证据；均有技术深度，值得入库
- raw/articles/：
  - 2026-06-02-agent-production-evaluation.md（新建，机器之心）
  - 2026-06-02-harness-engineering-anthropic-openai.md（新建，新智元）
- concepts/：
  - concepts/agent-production-evaluation.md（新建，Agent生产评估三层次）
  - concepts/harness-engineering.md（更新，追加五子系统架构 + Anthropic 9美元实验 + OpenAI Codex百万行实验 + 企业落地五步法）
- 跨链接：agent-production-evaluation → [[harness-engineering]], [[agent-drift-multi-agent-systems]], [[test-time-compute-scaling]], [[mcp]], [[superpowers]]
- WeRSS 情况：最近公号文章最新日期 2026-05-24（9天前），多为新闻简讯；仅此2篇有技术深度入库
- 总入库：2 raw + 1 新 concepts + 1 更新 concepts
