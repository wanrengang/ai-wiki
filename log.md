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

## [2026-05-03] create | 8 个 Entities 页面（第二批）
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
