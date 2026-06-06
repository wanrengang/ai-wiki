---
title: DeerFlow
created: 2026-03-08
updated: 2026-03-08
type: entity
tags: [agent, harness, bytedance, open-source, langchain, langgraph]
sources: [raw/articles/DeerFlow深度研究报告-字节跳动SuperAgent-Harness-2026-03-08.md]
confidence: high
---

# DeerFlow

> ByteDance 开源的 SuperAgent Harness，支持多代理编排、记忆系统和沙箱隔离。

## 概述

**DeerFlow** = **De**ep **E**xploration and **E**fficient **R**esearch **Flow**，是字节跳动开发的开源 SuperAgent 框架。2.0 版本从 Deep Research 工具重写为 SuperAgent Harness，支持任务分解、子代理并行执行和沙箱隔离。

## 关键事实

| 属性 | 值 |
|------|-----|
| 所有者 | ByteDance（字节跳动） |
| GitHub Stars | 25,712 |
| Forks | 3,037 |
| 创建时间 | 2025-05-07 |
| 最新版本 | 2.0（从零重写） |
| GitHub Trending | 2026年2月28日第1名 |
| 许可证 | MIT |

## 核心架构

```
DeerFlow 2.0
├─ LangChain — LLM 交互和链
├─ LangGraph — 多代理编排
├─ Python (后端) + TypeScript (前端)
└─ Docker (沙箱隔离)
```

### 核心组件

1. **Skills（技能系统）** — Markdown 文件定义工作流和最佳实践
2. **Tools（工具系统）** — Web Search/Fetch、文件操作、Bash 执行
3. **Sub-Agents（子代理）** — 并行执行，结构化结果汇聚
4. **Sandbox（沙箱）** — Docker 容器隔离，完整文件系统
5. **Memory（记忆）** — 会话内压缩 + 跨会话持久化
6. **Context Engineering** — 隔离的子代理上下文

## 内置技能

| 技能 | 功能 |
|------|------|
| Research | 深度研究和信息收集 |
| Report Generation | 自动生成报告 |
| Slide Creation | 创建演示文稿 |
| Web Page | 生成网页 |
| Image Generation | 生成图像 |
| Video Generation | 生成视频 |

## 技术特点

- **模型无关**：任何 OpenAI 兼容 API 的 LLM
- **MCP Server 集成**：HTTP/SSE + OAuth token flows
- **Python 嵌入式客户端**：无需运行完整 HTTP 服务
- **三种执行模式**：Local、Docker、Docker+Kubernetes

## 应用场景

- 深度研究任务（分钟到小时级）
- 数据管道构建
- 幻灯片生成
- 仪表板创建
- 自动化内容工作流

## 快速开始

```bash
git clone https://github.com/bytedance/deer-flow.git
cd deer-flow
make config          # 生成本地配置
# 编辑 config.yaml 配置模型
make docker-start   # 启动服务
# 访问 http://localhost:2026
```

## 相关链接

- GitHub: https://github.com/bytedance/deer-flow
- 官网: https://deerflow.tech


## 相关概念

- [[ai-agent]] — AI Agent 通用概念
- [[langchain]] — DeerFlow 的底层框架
- [[openclaw]] — 另一个开源 Agent Harness 对比
