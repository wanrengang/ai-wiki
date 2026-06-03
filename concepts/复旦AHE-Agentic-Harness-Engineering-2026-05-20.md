# 复旦 AHE：Agentic Harness Engineering 让 GPT-5.4 再涨 7 个点

> 来源：机器之心编辑部（2026-05-20）
> 原始链接：https://mp.weixin.qq.com/s/ObREWAzbx9znsfuz1r4ZuQ

## 核心成果

来自**复旦大学、北京大学、上海奇绩智峰**的团队提出 **Agentic Harness Engineering (AHE)**，一套可观测性驱动的 Harness 自动优化方法。

| 成果 | 数据 |
|------|------|
| GPT-5.4 在 Terminal-Bench 2 | 69.7 → 77.0（涨 7.3 点） |
| GPT-5.5 + AHE | Leaderboard 全球第三 |

- 论文：arxiv.org/abs/2604.25850
- 代码：github.com/china-qijizhifeng/agentic-Harness-engineering
- 博客：dawning-road.github.io/blog/agentic-Harness-engineering

---

## 核心问题

Harness Engineering 的迭代循环高度依赖人工经验，但模型以月为单位进化、任务场景往长尾分布发展——**哪些部分可以被自动化？如何让 Harness 自动从经验中学习并改进？**

---

## AHE 架构：三个角色

| 角色 | 职责 |
|------|------|
| **Coding Agent** | 运行测试 |
| **Agent Debugger** | 整理轨迹，把 10M token 量级 raw trace 提炼成分层可溯源的多维反馈意见 |
| **Evolve Agent** | 基于 git 溯源的组件历史和反馈结果，构建证据驱动的修改链路 |

---

## 可观测体系三部分

### 1. 组件可观测性：解耦的"声明式 Harness"

Harness 拆成七种正交文件级组件：
- System Prompt
- Tool Description
- Tool Implementation
- Middleware
- Skill
- Sub-agent Config
- Long-term Memory

通过 Git 版本管理，每次变更可追溯、可审计、可回滚。Coding Agent 从"零先验"极简形态起步（只有一个 run_shell_command 工具），确保每次新增组件都能被干净归因。

### 2. 经验可观测性：Agent Debugger 把轨迹变成可消费资产

- **底层**：完整原始轨迹
- **中层**：Cleaner 去除重复工具输出
- **上层**：QA Sub-agent 针对每题多次 rollout 结果自动切换提问策略
- **汇聚**：约 10K Token 概览报告交给 Evolve Agent

本质是**渐进式披露**——10M 级别数据变成可并发、可消费、可审计的经验资产。

### 3. 决策可观测性：证据驱动修改

规则：
- 只能修改 workspace 内 Harness 组件文件，评测框架/LLM 配置/原始 System Prompt 均为**只读**
- 每次修改必须附带"变更清单"：失败证据 → 推断根因 → 针对性修改方案 → 自我预测
- 预测正确的修改保留，预测错误的自主回滚

> 每一次 Harness 变动都不再是工程师的直觉、抽象经验，而是一条可被下一轮实验所证伪的假说。Harness 进化由此从艺术走向工程，从经验走向科学。

---

## 关键发现：事实比策略更可迁移

消融实验结果：

| 组件 | 效果 |
|------|------|
| **Memory 单独** | 恢复全局增幅的 95% 以上 |
| Tool | 中等难度题目上提升显著 |
| System Prompt 单独迁移 | 性能反而下降 |

**原因**：Prompt 的语义是"策略性的"（你应该这样做），而 Memory 和 Tool 是"事实性的"（这里有一段可复用代码）。事实比策略迁移性好。

---

## 经验教训

| 阶段 | 问题 | 解决方案 |
|------|------|----------|
| 初期（30题/10轮） | 过小题集导致针对性 hack，修一个坏一个 | 扩到 89 题全集 |
| 中期（加方法论指导） | 人工引入的行为先验成了进化僵化之源，75.3% 早早触顶 | 删除所有行为指导，只保留证据驱动 |
| 最终版 | - | 每题跑两次，通过 partial-pass diff 定位诊断信号 |

最终修改分布：Middleware 37% + Tool 48% + Prompt 10%，没有任何层级单独超过一半。

---

## 核心启示

> 当模型足够强，搭建一个结构化的、可观测的演化环境，比直接开发 Harness 更重要。搭建好观测体系（让 Evolve Agent 能访问组件、轨迹、反馈），然后在全量数据上运行测试，就足够演化出有竞争力的 Harness。无需替 Agent 思考任何方法论，只是给它一个清晰的 workspace、明确的修改接口和高质量的反馈信号，Evolve Agent 的行为便自动向真实工程师收敛。

---

## 泛化能力

- **跨任务泛化**：Terminal-Bench 2 上演化的 Harness 迁移到 SWE-Bench Verified，以更少 Token 消耗实现更高成功率
- **跨模型泛化**：GPT-5.4 演化得到的 Harness 配到 Qwen-3.6-Plus、Gemini-3.1-Flash、DeepSeek-V4 上，不做任何再演化，三种模型均获得 +5.1 到 +10.1 个百分点的显著提升，且模型越弱提升越大

---

标签：#Harness #AHE #Agent工程 #复旦大学 #可观测性 #GPT-5.4 #Terminal-Bench
