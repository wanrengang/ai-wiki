---
title: SwarmHarness
created: 2026-05-29
updated: 2026-05-29
type: concept
tags: [agent, architecture, open-source, research]
sources: [raw/articles/2026-05-29-swarmharness-decentralized-ai-agent-network.md]
confidence: high
---

# SwarmHarness

去中心化算力激励协议，使 HarnessAPI skill 节点自组织为计算蜂群，无需中心权威或区块链基础设施。

## 概述

**问题：** 全球大量 GPU 算力（个人工作站、闲置服务器、边缘设备）未被利用，原因是缺乏激励对齐的协议让算力所有者安全获利。

**方案：** SwarmHarness 通过三个组件实现无中心权威的去中心化算力共享：
- **SwarmRegistry**：基于 DHT 的对等发现与能力广告
- **SwarmRouter**：基于 utility function 的任务分发（capability/load/latency/trust）
- **SwarmCredit**：Shapley 值近似的激励机制，无需区块链

## 核心机制

### SwarmRegistry（DHT 对等发现）
- 节点通过分布式哈希表广告自身技能、负载、延迟、信任评分
- 无中心服务器，类似 BitTorrent 的对等网络
- 能力广告格式：`{skill_type, load_score, latency_estimate, trust_score}`

### SwarmRouter（Utility-scoring 任务分发）
- **Utility function**：`U = w1·capability + w2·(1-load) + w3·(1-latency) + w4·trust`
- 路由信号作为"数字信息素"——高频使用模式自我强化，形成类似生物蜂群的信息素轨迹
- 冷启动解决方案：通过"证明贡献创世纪机制"积累初始信任

### SwarmCredit（Shapley 值激励）
- 基于合作博弈的 Shapley 值近似，实时多节点归因
- **积分经济**：节点通过服务任务赚取积分，通过提交任务消耗积分
- **自我调节**：闲置节点积分消耗后失去路由优先级，自然退出
- **抗攻击**：Genesis 机制要求新节点首先贡献有效工作

## 与 HarnessAPI 的关系

SwarmHarness 将 HarnessAPI（Jose, 2026）从**单节点架构**扩展到**去中心化多节点协议**，保留对现有 HarnessAPI 部署的完整向后兼容。

HarnessAPI 的核心创新：类型化 skill 文件夹同时暴露为 HTTP 端点和 MCP 工具，消除 LLM 工具部署的双重维护负担。

## 定位与愿景

**核心定位：** 自主分布式 AI Agent 网络的基础原语

未来愿景：Agent 在互联网上雇佣算力、路由子任务、结算付款，无需任何层级的可信中介——实现真正的无信任（trustless）分布式 AI 基础设施。

## 与现有框架对比

| 框架 | 激励层 | 去中心化 | 适用场景 |
|------|--------|----------|----------|
| BOINC | 无 | 是 | 科学计算志愿计算 |
| Petals | 无 | 是 | LLM 协作推理 |
| Golem/BrokerChain | 是 | 是 | 区块链算力市场 |
| 云市场（AWS等） | 是 | 否 | 中心化商业算力 |
| **SwarmHarness** | **是** | **是** | **Agent 算力自组织** |

## 相关概念

[[harness-engineering]] — PE→CE→HE 演进，HarnessAPI 是 HE 的核心组件

[[mcp]] — MCP 协议是 HarnessAPI skill 暴露为工具的标准接口

[[openclaw]] — OpenClaw 的 Agent 编排与 SwarmHarness 的去中心化算力路由可互补

[[agent-drift-multi-agent-systems]] — 多智能体系统长期运行行为退化问题

[[superpowers]] — AI 编程增强系统，与 SwarmHarness 的算力层形成应用+基础设施关系