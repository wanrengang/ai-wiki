# Prompt → Context → Harness → Loop：大模型应用开发范式的四次跃迁

**来源**：AI科技观察 / 小番茄学AI
**日期**：2026-06-17 18:05
**链接**：https://mp.weixin.qq.com/s/c66CEmeH63GKAdREfRT81g

---

## 核心概述

从"写好一句话"到"设计一个自运行的智能系统"，AI 开发的范式在过去两年内升维了三次。

> **Boris Cherny（Anthropic Claude Code 负责人）**：
> "I don't prompt Claude anymore. I have loops running that prompt Claude and figure out what to do. My job is to write loops."
> （我不再手动给 Claude 写提示词了。我有循环在运行，它们负责提示 Claude 并决定做什么。我的工作是设计这些循环。）

---

## 四层演进路线图

| 层级 | 关注点 | 核心问题 | 代表年份 |
|------|--------|----------|---------|
| **Prompt Engineering** | 单次交互 | 我怎么措辞？ | 2022-2024 |
| **Context Engineering** | 会话级信息架构 | 模型该看到什么？ | 2025 |
| **Harness Engineering** | Agent 运行时系统 | 模型出错时怎么办？ | 2026 |
| **Loop Engineering** | 自主运行循环 | 如何让系统自己跑？ | 2026.6 |

---

## 1. Prompt Engineering（2022-2024）

**核心问题**：我怎么措辞，模型才能理解我的意图？

**技术栈**：
- 角色设定（"你是一个资深软件工程师"）
- 少样本示例（Few-shot）
- 思维链引导（"让我们一步一步思考"）
- 输出格式约束（"以 JSON 格式返回"）

**根本局限**：优化的是一次交互，无法解决：
- 模型忘了前面说过什么
- 工具返回的信息淹没关键指令
- 上下文窗口被历史消息撑爆

---

## 2. Context Engineering（2025）

**定义**（Andrej Karpathy）：在模型推理的每一步，精确填充上下文窗口的艺术与科学。

**五个维度**：

| 维度 | 说明 |
|------|------|
| **检索质量** | 不是"塞更多文档"，而是"在正确时刻塞对的那一段" |
| **状态管理** | 什么时候压缩对话历史？什么时候写入持久记忆？什么时候丢弃？ |
| **工具设计** | 工具的返回值是上下文的一部分——返回精炼的结构化数据就是做 context engineering |
| **Token 预算** | 每个推理步骤，哪些信息有资格占用宝贵的 token 空间 |
| **上下文布局** | 静态内容放窗口前端（利用 prompt caching），动态内容放末尾 |

**标志性转变**：模型的能力不再是瓶颈，瓶颈变成了你给模型喂什么信息。

---

## 3. Harness Engineering（2026）

**公式**：Agent = Model + Harness

**Mitchell Hashimoto**：模型是大脑，Harness 是包裹在大脑外面的一切——工具接口、权限边界、重试策略、评估机制、人类审批闸门、可观测性、记忆系统、沙箱环境。

**Atlan**：Harness Engineering 之于上下文工程，如同上下文工程之于提示词工程——不是替代，是包含。

**OpenAI Codex 团队 Ryan Lopopolo**："Agents aren't hard; the Harness is hard."（Agent 本身不难，难的是 Harness。）

**本质**：都是软件工程问题：
- 模型输出不靠谱？加验证层
- Agent 陷入循环？加超时和退出条件
- 操作太危险？加权限沙箱和人类审批
- 运行太久 token 不够？自动压缩上下文
- 多个 Agent 之间需要协同？设计消息传递协议

**Chad Fowler** 称之为"Rigor Relocation"——工程严谨性没有消失，它迁移了。就像当年从瀑布式设计文档到敏捷自动化测试，严谨性从前期文档搬到了测试套件里。现在，严谨性从手写代码搬到了 Harness 设计里。

---

## 4. Loop Engineering（2026.6）

**核心思想**：你不再是一个一个 prompt 的操作者。你设计一个系统，这个系统来发现工作、分配任务、检查结果、记录进度、决定下一步——你只定义"什么是完成"和"什么情况下停"。

**五个构建块**：

| 构建块 | 说明 |
|--------|------|
| **调度触发器** | 定时任务（cron）、事件驱动（webhook、文件变更）、或者 `/loop` 这样的内置命令 |
| **隔离执行** | 每个任务在独立的 git worktree 或沙箱中运行，互不干扰 |
| **验证闸门** | 一个独立 Agent（或规则引擎）审核主 Agent 的工作，通过才能合入 |
| **外部状态** | 用文件（`LOOP.md`、`STATE.json`）而非上下文窗口记录进度——Agent 会忘，仓库不会 |
| **多 Agent 分工** | 探索者、实现者、验证者各司其职，通过结构化产物（spec 文件、commit、PR body）交接 |

**Addy Osmani**：Loop Engineering 是 Harness Engineering 中最关键、最容易被忽视的子领域。

---

## 四层嵌套关系

每一次向外扩展，都不是对上一层的否定，而是**将其吸收为自己的子系统**：
- Prompt Engineering 没有死——它成了 Context Engineering 的一个组件
- Context Engineering 是 Harness 设计的信息输入层
- Loop Engineering 是 Harness 从"手动挡"到"自动挡"的升级

---

## 核心洞察

> AI 应用开发的抽象层级在持续上移——就像编程语言从汇编到 C 到 Python，每次上移都释放了巨大的生产力杠杆。

**生产力差距**：
- Prompt Engineering 时代：高手和新手的差距是 **2 倍**
- Loop Engineering 时代：设计自运行系统 vs 仍在敲 enter，差距是 **100 倍**

**原因**：前者在睡觉的时候，系统还在工作。

> 你不能再用打字员的思维，做架构师的事。
