---
source_url: https://mp.weixin.qq.com/s/Z8IOHlufZrZvgF4be5X-lA
ingested: 2026-05-29
sha256: a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2
---
# DeepSeek陈德里与两个AI，合写了一篇论文

原创 关注AI的 机器之心 2026年5月27日 10:18 北京

**摘要：** DeepSeek 研究员陈德里（Deli Chen）发布 46 页综述论文 "From Copilots to Colleagues: A Survey of Autonomous Research Agents"，由 AI Agent 深度参与完成（1% 人类，99% AI）。论文迭代 6 轮，108 轮 Agent 交互，消耗约 64.8 万 tokens，耗时 6 天，95+ 篇参考文献。核心贡献：L1-L5 自主等级分类体系。

## 基本信息

- **第一作者**：陈德里（Deli Chen），DeepSeek V1/V2/V3/V4/R1、DeepSeek-Coder、DeepSeek-MoE 核心贡献者
- **两位 AI 合著者**：DeepSeek-V4-Pro（文字）+ GPT-Image2（图像）
- **Deli AutoResearch SKILL**：陈德里开发的自主科研智能体框架
- **论文地址**：https://victorchen96.github.io/auto_research_survey.pdf
- **博客地址**：https://victorchen96.github.io/

## 核心产出数据

- 6 轮迭代（V1: 4轮，V2: 1轮，V3: 1轮）
- V1 初稿耗时 76 分钟，总耗时 6 天
- 108 轮 Agent 交互，消耗约 64.8 万 tokens
- LaTeX 共 2234 行
- 103 篇参考文献，全部已核验
- 46 页，文件大小 538KB
- 7 张图 + 4 张表
- 人类实际"动脑"总 CPU 时间：不到 2 小时

## 核心贡献一：L1-L5 自主等级分类体系

| 等级 | 名称 | 描述 | 代表系统 |
|------|------|------|----------|
| L1 | 自动补全 | AI 预测下一行代码，人类掌控方向 | GitHub Copilot |
| L2 | 任务执行 | AI 分解任务、调用工具，每步需人类批准 | ChatGPT/Claude 日常交互 |
| L3 | 多步自主（设检查点） | AI 在检查节点前独立执行数十步操作 | Claude Code, Cursor Agent |
| L4 | 端到端全自动 | 独立工作数小时至数天，产出完整成果 | Devin, SWE-Agent, AI Scientist |
| L5 | 自主设定研究议程 | 自主选择研究问题、分配资源、跨月积累知识 | 愿景，Google Co-Scientist、DeepMind FunSearch 有部分能力 |

## 核心贡献二：四种架构模式

1. **单智能体循环**：简单可控，但复杂任务易触及上限
2. **多智能体协作**：分工扮演不同角色，MetaGPT 将 SOP 编码进协作，任务完成率 67%→100%
3. **层级编排**：管理者-执行者模式，Claude Code 采用，全局状态和高层规划由主智能体维持
4. **工具增强执行**：ChemCrow 集成 18 种化学专用工具，化学问题正确率 30%→75%

## 核心贡献三：六大未解难题

1. **认知循环陷阱**：智能体陷入死循环，目前无通用系统性解决方案
2. **上下文窗口限制**：跨会话"研究记忆"难以实现
3. **新颖性评估**：如何判断 AI 产出的研究成果是否真正新颖
4. **可重现性危机**：基准测试性能标准差动辄 5%-15%
5. **安全与伦理**：有益和有害的能力难以分离
6. **成本与可及性**：最强大基础模型仍是专有的、昂贵的，加剧科研不平等

## 背景：SWE-bench 18 个月从 5% 到 70%

- AI 正在从"研究工具"变成"研究者"本身
- 18 个月内 AI 解决真实 GitHub 问题比率从不足 5% 攀升至 70% 以上
- 有系统以每篇 15 美元成本产出完整学术论文并通过人类初审
- 有系统在无人引导情况下发现超越已知边界的新数学构造

## 结语

"L5 自主研究 —— 能够自主制定长期研究议程的智能体 —— 是一个『何时』而非『是否』的问题。研究社区的任务是确保这一转变伴随着充分的理解、适当的保障，以及公平的收益分配。"