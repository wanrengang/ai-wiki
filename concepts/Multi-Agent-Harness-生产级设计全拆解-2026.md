# 从零设计生产级 Multi-Agent Harness：架构、评估、记忆、成本与 MCP 工具接入全拆解

> 来源：腾讯云开发者｜李伟山  
> 发布时间：2026年5月13日  
> 原文链接：https://mp.weixin.qq.com/s/JPhcyDc4JwRmnMQ-76A-FQ

## 核心观点

**Multi-Agent 在 Demo 阶段"拼几个 Agent"就够了，而生产阶段"没有 Harness 寸步难行"。**

> Prompt 是台词，Agent 是演员，工具是道具，模型是大脑，而 Harness 是整个舞台的导演、灯光、调度系统、安全规章和票务系统。
> 
> 没有舞台系统，演员再好也只是即兴表演；有了舞台系统，才能稳定地一晚演两场，连演一年。

---

## 01 Harness 是什么

Multi-Agent Harness = Agent 的"操作系统"。

- 不只是 Prompt 模板：Prompt 解决"怎么让模型理解任务"，Harness 解决"怎么让模型可靠地完成任务"
- 不只是 Orchestrator：Orchestrator 解决"顺序"，Harness 还要解决"资源、记忆、成本、安全、可观测"
- 不只是 Agent Framework：框架是积木，Harness 是把积木拼成生产建筑的工程方案

---

## 02 架构编排

**核心原则：Agent 负责局部智能，Harness 负责全局控制。**

Orchestrator 必须独占以下五项决策权：

1. **任务生命周期**：每个任务从创建、规划、执行、审查、完成、失败都要有明确的状态机
2. **执行计划裁决**：计划可以来自静态 DAG 或 Planner Agent，但计划一旦生成，必须由 Orchestrator 接管
3. **Agent 路由**：结合任务类型、Agent 能力、权限、历史质量评分
4. **失败处理**：某个 Agent 失败后是重试、降级、跳过还是终止，绝不能让出错 Agent 自己说了算
5. **硬终止条件**：max_steps、max_tokens、max_duration、max_tool_calls 四道硬闸

**Planner 应输出"声明式计划"，而不是"命令式调用"。**

- 声明式：`{step: 1, intent: "research", agent: "researcher", input: "..."}`
- 命令式：直接 `await researcher.run("...")`

> 别让 Agent 开车，让 Agent 当导航。

---

## 03 工具治理

**工具不是函数调用，而是生产资源的对外授权点。**

一个合格的 Tool Registry，每个工具都至少要登记九项元信息：
1. 工具名称（唯一标识）
2. 工具描述（给 LLM 看的说明）
3. 输入参数 JSON Schema（用于校验）
4. 允许调用的 Agent 列表（RBAC）
5. 调用超时与速率限制
6. 风险等级（低/中/高）
7. 是否需要人工确认
8. 输出结果结构
9. 审计日志策略（保存什么、保留多久、谁能看）

> 哪怕你只有 3 个工具，也从第一天起强制走 Tool Registry。先有规矩，后扩规模。

---

## 04 状态与记忆

把"状态"和"记忆"分开：

**状态（State）**：当前任务运行所需的数据，生命周期短，关心一致性
- Working State：当前步骤的临时上下文，任务结束即丢
- Session State：一次会话里多个 Agent 共享的信息，放 Redis，设 TTL
- Execution Log：不可变执行日志，用于审计、回放、评估

**记忆（Memory）**：跨任务复用的经验和知识，生命周期长，关心相关性
- Episodic Memory（事件记忆）：踩过的坑、用户偏好、某类问题处理经验
- Semantic Memory（语义记忆）：领域概念、业务规则、工具约束

### 两个关键设计点

**第一：注入时机（Retrieval Timing）**
- 任务前自动注入：简单稳定但费 Token
- 按需检索：省钱但 Agent 可能忘记调用
- **生产推荐：混合模式**——前置注入少量高置信记忆 + 提供 `memory_search` 工具供 Agent 主动调用

**第二：遗忘机制（Memory Forgetting）**
- 低分记忆 → 直接删除
- 中分记忆 → 压缩为摘要
- 高分记忆 → 保留原文

> 记忆不是仓库，而是花园。需要定期修剪。

---

## 05 评估体系

**不要只看答案，要看轨迹。**

生产级 Eval Pipeline 分四层：

1. **Component Eval（组件评估）**：单 Agent 是否选对工具、参数是否合规、输出是否符合角色职责
2. **Trajectory Eval（轨迹评估）**：步骤是否必要、顺序是否合理、是否重复调用、是否陷入循环（Multi-Agent 最大的创新点）
3. **Task Completion Eval（任务完成度）**：是否满足用户目标、是否覆盖必要信息、是否存在事实错误
4. **End-to-End Eval（端到端业务效果）**：用户是否采纳、人工返工率、处理时长、单位任务成本

**LLM-as-Judge 不是万能药**——对事实正确性、代码可运行性、SQL 结果、权限合规，应该优先用确定性检查。

成熟的 Eval 必然是混合的：
- 单元测试检查代码
- Schema 校验结构化输出
- 规则引擎检查安全约束
- 检索对齐校验引用来源
- LLM-as-Judge 评开放式表达
- 人工抽检校准 Judge 偏差
- 线上反馈验证业务效果

> **Eval 必须进入 CI。** 对 Agent 系统来说，Prompt 就是代码，工具 Schema 就是接口，执行轨迹就是日志，Eval 就是测试体系。

---

## 06 成本控制

**Token Budget 是生产级 Agent 的生命线。**

Multi-Agent 烧钱的原因：
- 每个 Agent 都有 System Prompt
- 每个 Agent 都需要上下文
- 工具结果会被塞回模型
- 多轮协作让历史不断复制膨胀

### 三个核心策略

**策略一：Model Routing（模型路由）**
- 分类、摘要、格式转换 → 小模型即可
- 复杂推理、最终合成 → 用强模型
- 高风险审查 → 强模型 + 规则校验双保险
- 低价值重试 → 禁止使用高价模型

**策略二：Context Compression（上下文压缩）**
- 保留最近几轮原文 + 把更早历史压缩成结构化摘要
- 事实型任务必须保留原始引用
- 合规型任务关键证据不可压缩

**策略三：Budget 分级降级**
- 绿区（>50%）：正常执行
- 黄区（20%–50%）：压缩上下文
- 红区（5%–20%）：切小模型 + 跳过 CoT
- 熔断区（<5%）：强制收束，返回 partial result

**必须监控的指标：**
- 单任务 Token 总量
- 单 Agent Token 占比
- 工具结果 Token 占比
- 重试 Token 占比
- 不同路由策略下的成本与成功率
- 预算熔断次数
- **单位业务结果成本**（每完成一个合格任务多少钱）

> 当你能精准回答最后一项时，Agent 系统才真正进入了可运营阶段。

---

## 07 MCP 工具接入

MCP（Model Context Protocol）= 模型上下文协议

类比：**MCP 之于 AI 工具，如同 USB-C 之于充电接口。**

对 Multi-Agent Harness 的意义：
- 快速扩展能力：一键接入文件系统、数据库、Git、Slack、Jira、内部 API 等
- 生态可复用：业界形成的 MCP Server 可以直接拿来用
- 解耦工具与模型：工具实现不绑定特定 LLM

**但标准化不等于安全。工具越容易接入，越需要 Harness 在中间做安全网关。**

最佳实践：
1. 永远不要把 MCP Server 直接暴露给 Agent，必须经过 Tool Registry
2. 给每个 MCP Server 单独配额
3. 对工具做白名单而不是黑名单
4. 高风险工具（文件写入、删除、代码执行、数据库写、外部支付）一律走 Human-in-the-Loop
5. 所有 MCP 调用都要打 Trace

> MCP 让工具接入变得便宜，Harness 让工具调用变得可信。两者必须搭配，不可偏废。

---

## 08 可观测性与落地路线

### 可观测性

传统后端出问题时看日志、指标、链路。Agent 系统更需要这些，因为 Agent 的错误往往不是异常，而是过程偏移：
- 可能调用了错误工具
- 可能读取了错误记忆
- 可能误解了用户目标
- 可能因为压缩丢了关键约束
- 可能因为预算不足提前收束
- 可能因为路由用了能力不够的小模型

### 落地路线：三阶段演进

**Phase 1 — MVP**
最小 Orchestrator + Tool Registry + 简单状态 + 基础 Trace + 评估数据集。不要一开始就上动态 Planner、十个 Agent、复杂长期记忆。先把一条链路跑稳。

**Phase 2 — Hardening**
增加 Budget、权限、重试、压缩、轨迹评估、审计、回归测试。重点解决"为什么错、哪里贵、哪里慢、哪里不安全"。

**Phase 3 — Scale**
引入分布式队列、多租户隔离、动态模型路由、Agent 质量排行榜、A/B 测试、长期记忆治理、统一 MCP 接入平台、成本看板。

### 技术选型建议

- **小团队**：LangGraph 或自研轻量状态机 + FastAPI + Redis + PostgreSQL/pgvector + Langfuse/OpenTelemetry + LiteLLM 网关
- **企业团队**：必须更重视权限、审计、多租户、成本中心、数据治理。MCP 作为接入标准，但不允许直连 Agent
- **研究团队**：可以探索动态 Planner、自反思、自动 Eval、长期记忆压缩，但务必区分研究效果和生产 SLA

---

## 核心结论

> 如果说 Prompt 是 Agent 的台词，工具是它的手脚，模型是它的大脑，那么 Harness 就是它的骨架、神经系统和安全带。
> 
> 没有 Harness，Multi-Agent 只是热闹；有了 Harness，Agent 才可能成为生产力。

**十个关键问题：**
1. 任务怎么进来？
2. 谁负责拆解？
3. 谁负责调度？
4. 工具怎么接？
5. 状态放哪里？
6. 记忆怎么检索？
7. 预算怎么控制？
8. 轨迹怎么评估？
9. 失败怎么处理？
10. 审计怎么保留？

把这十个问题回答清楚，你就已经越过了大多数 Agent Demo 的边界。

---

*标签：#Multi-Agent #Harness #Agent架构 #MCP #AI工程化 #RAG #成本控制 #评估体系*
