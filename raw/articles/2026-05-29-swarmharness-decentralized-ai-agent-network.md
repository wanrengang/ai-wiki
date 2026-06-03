---
source_url: https://arxiv.org/abs/2605.28764
ingested: 2026-05-29
sha256: e9b2c3d4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3
---

# SwarmHarness: Skill-Based Task Routing via Decentralized Incentive-Aligned AI Agent Networks

**arXiv:** 2605.28764v1 [cs.AI] | 2026年5月27日

**作者:** Edwin Jose（Western Michigan University）

---

## Abstract

闲置 GPU 算力（个人工作站、夜间闲置服务器、边缘设备）大量未被利用，原因是没有激励对齐的协议让算力所有者安全获利。现有方案要么需要可信中心协调（云市场），要么需要沉重的区块链基础设施（Golem、BrokerChain），要么完全缺少激励层（BOINC、Petals）。

**SwarmHarness** 提出一种去中心化协议，使 HarnessAPI skill 节点自组织为计算蜂群，无任何中心权威。三个组件：
- **SwarmRegistry**：基于 DHT（分布式哈希表）的对等发现与能力广告
- **SwarmRouter**：基于 utility function 的任务分发（capability、load、latency、trust）
- **SwarmCredit**：Shapley 值近似的激励机制，无需区块链对计算贡献者进行奖励分配

节点通过服务任务获得积分，通过提交任务消耗积分；从不贡献的空闲节点积分消耗后失去路由优先级，形成自我调节的参与经济。随着节点专精高奖励技能，路由信号作为数字信息素，网络展现出类似生物蜂群的涌现集体智能。

**核心定位：** 自主分布式 AI Agent 网络的基础原语——Agent 雇佣算力、路由子任务、结算积分，无需任何人类中介。

---

## 1 Introduction 核心要点

- **问题：** 数万亿 GPU 小时每年被闲置。研究人员工作站夜间空闲；边缘服务器等待推理请求；学生游戏 GPU 在训练间隙闲置。同时，组织排队等待昂贵的云算力，而附近闲置机器几秒就能完成。
- **障碍不是技术**，而是**激励**：所有者没有安全、自动化的机制来暴露闲置算力、追踪贡献、获取合理报酬。
- **HarnessAPI（Jose, 2026）**：skill-first 框架，将类型化 skill 文件夹同时暴露为 HTTP 端点和 MCP 工具，消除 LLM 工具部署的双重维护负担。每个节点是自主 Agent：知道自身技能、接受任务、流式返回结果。
- **核心问题：** 当数千个 HarnessAPI 节点运行在不同人拥有的硬件上会发生什么？Agent 可以雇佣算力委托子任务，结算付款，无需任何人类或云中介。

---

## 2 Related Work

### 2.1 Agent Harness 框架
- **ReAct 范式（Yao et al., 2022）**：在 LLM Agent 中交替推理痕迹和工具使用动作，在交互任务上比思维链提升 34%
- **后续框架（LangChain、AutoGen、CrewAI）**：构建编排层但仍保留中心协调器路由任务
- **AgentMesh（Khanzadeh, 2025）**：分解为 Planner、Coder、Debugger、Reviewer Agent，错误传播和上下文扩展是主要局限
- **HarnessAPI（Jose, 2026）**：反转框架依赖——类型化 skill 文件夹成为单一真相来源，自动派生 HTTP 端点和 MCP 工具

### 2.2 去中心化计算共享
- **BOINC（Anderson, 2019）**：经典志愿者计算平台，展示闲置消费级硬件可以被大规模利用；超过 500 万志愿者计算机，但无货币激励
- **Petals（Borzunov et al., 2022）**：类 BitTorrent 的 LLM 推理，分布式 BLOOM-176B 到消费级 GPU，无激励层，允许免费搭便车
- **Golem / BrokerChain**：区块链市场有激励但链上交易成本和延迟与交互推理不兼容

---

## 3 System Architecture

### 3.1 SwarmNode
每个 HarnessAPI 节点扩展为 SwarmNode：
- 知道自身技能列表
- 接受任务并流式返回结果
- 通过 SwarmRegistry 广告自身能力
- 通过 SwarmRouter 接收任务路由

### 3.2 SwarmRegistry（基于 DHT）
- **对等发现**：节点注册到 DHT，广告其技能、负载、延迟、信任评分
- **无中心服务器**：类似 BitTorrent 的对等网络，节点互相发现
- **能力广告格式**：`{skill_type, load_score, latency_estimate, trust_score}`

### 3.3 SwarmRouter（utility-scoring 任务分发）
- **Utility function**：`U = w1·capability + w2·(1-load) + w3·(1-latency) + w4·trust`
- **路由信号作为数字信息素**：高频使用的路由模式自我强化，形成类似生物蜂群的信息素轨迹
- **冷启动解决方案**：新节点通过"证明贡献创世纪机制"积累初始信任

### 3.4 SwarmCredit（Shapley-value 激励）
- **Shapley 值近似**：实时多节点归因，考虑每个节点的边际贡献
- **信任衰减反馈**：长期不贡献的节点信任评分衰减，防止坐食
- **积分经济**：节点通过服务任务赚取积分，通过提交任务消耗积分
- **抗冷启动攻击**：Genesis 机制要求新节点首先贡献有效工作才能开始赚取积分

---

## 4 SwarmCredit Attribution Algorithm

### 核心机制
1. **任务提交**：Agent 提交任务并支付积分
2. **节点选择**：SwarmRouter 基于 utility function 选择最优节点执行
3. **执行验证**：结果通过某种验证机制确认（防止作弊）
4. **积分分配**：Shapley 值算法计算每个节点的边际贡献并分配积分

### 关键属性
- **无区块链**：通过本地账本和拜占庭容错机制实现去中心化信任
- **抗攻击**：冷启动抗 griefing 机制，新节点无法通过假任务刷积分
- **自我调节**：闲置节点积分持续消耗，优先路由权下降，形成自然退出机制

---

## 5 Feasibility and Open Challenges

### 部署可行性
- 可在现有 HarnessAPI 部署上直接扩展
- 引导问题：通过"证明贡献"机制解决冷启动

### 安全面
- 恶意节点提交假结果
- 信任评分 sybil 攻击
- 积分通胀

### 短期价值
- 闲置 GPU 变现
- 无需信任中心的算力市场

### 长期愿景
- **自主分布式 AI Agent 网络的基础原语**
- Agent 在互联网上运营，无需任何层级的可信中介

---

## 6 Conclusion

SwarmHarness 将闲置 GPU 算力转化为可自组织、自激励的去中心化计算蜂群。三个组件（DHT SwarmRegistry、utility-scoring SwarmRouter、Shapley-value SwarmCredit）共同实现无中心权威的算力共享激励。

**定位为未来自主分布式 AI Agent 网络的基础原语**，为 AI Agent 在互联网规模上运营铺平道路。

---

## 核心创新总结

| 组件 | 技术 | 核心创新 |
|------|------|----------|
| SwarmRegistry | DHT | 去中心化对等发现，能力广告 |
| SwarmRouter | Utility function | 多维路由（capability/load/latency/trust），数字信息素 |
| SwarmCredit | Shapley value approximation | 无区块链的公平激励归因，抗冷启动攻击 |

**与 HarnessAPI 的关系：** SwarmHarness 是 HarnessAPI 从单节点架构扩展到去中心化多节点协议，保留对现有 HarnessAPI 部署的完整向后兼容。

**与 OpenClaw/Agent 生态的关系：** 可类比为 Agent 算力层的"Uber"——让全球闲置算力被 Agent 按需雇佣，形成去中心化算力市场。

---

**来源：** [arXiv Abstract](https://arxiv.org/abs/2605.28764) | [arXiv HTML](https://arxiv.org/html/2605.28764v1) | Western Michigan University