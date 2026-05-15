---
title: Ralph Loop 三厂大战：Goal Mode 的三条路径
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [agent, coding, architecture]
sources: [raw/articles/2026-05-14-ralph-loop-goal-mode-war.md]
confidence: high
---

# Ralph Loop 三厂大战：Goal Mode 的三条路径

## 起源

2026 年 4 月，澳大利亚牧羊大叔 Geoffrey Huntley 用三行 bash 写了一个无限循环脚本，让 Agent 持续工作直到任务完成：

```bash
while :; do while do cat PROMPT.md | claude-code --continue cat continue done done
```

命名为 **Ralph Loop**（致敬《辛普森一家》里永不放弃的 Ralph Wiggum）。

11 天内，OpenAI、Anthropic、Hermes 三家顶级实验室不约而同地将这个三行脚本写进了官方产品。

## 三厂实现对比

### OpenAI Codex：持久化目标存储

- `/goal` 是持久化工作流对象，存在本地 app-server 状态层
- 跨终端关闭、重启不丢，下次自动接上
- 结构化 `update_goal` 工具汇报进度，token 耗尽触发「软着陆」
- 有人用此功能连续跑 14 小时，中间睡 5 小时，回来从断点续跑完成项目
- **定位**：工程化、干净、克制

### Hermes Agent：多智能体团队协作

- `/goal` 只是冰山一角，真正重头戏是**多智能体看板系统**
- 看板底层是本地 SQLite，跨重启不丢
- 任务卡片自动拆分为多个子任务，分配给不同 Agent worker（独立 OS 进程）
- 五层防烂尾机制：心跳检测、僵尸回收、退出拦截、幻觉拦截（验证代码是否真的落盘）、重试预算
- **定位**：从单 Agent 问题升级为团队协作问题，纵向深入 + 横向铺开

### Claude Code：裁判模型验收

- `/goal` 是 session 级别的 **Stop Hook**
- 设定完成条件（如「test 目录下所有测试通过且 lint 无报错」）
- 关键设计：**做事的人和验收的人不能是同一个**
- 裁判模型（默认 Haiku）接收对话记录 + 完成条件，判断是否完成
- 若未完成，返回具体理由注入下一轮上下文；若完成，目标自动清除
- 裁判模型不调用任何工具，只看对话产出内容
- **定位**：巧妙的信任分离设计

## 战略意义

三条路径恰好覆盖 ASI 决赛三个生态入口：

- Claude Code → Anthropic
- Codex → OpenAI
- Hermes → DeepSeek V4 + 双方模型

争的是**工作流入口**：谁的 Agent 先让开发者养成「设完目标就走开」的习惯，谁就锁死工作流护城河。因为习惯一旦形成，迁移成本指数级上升。

## 相关概念

- [[hermes-agent]] — Hermes Agent 的五层防烂尾机制
- [[openai-frontier]] — Codex 是 OpenAI Frontier 的 Agent 编程能力
- [[test-time-compute-scaling]] — Goal Mode 是 Test-Time Scaling 的工程落地
- [[harness-engineering]] — 从 PE → CE → HE 的演进中，Goal Mode 属于 CE 层的执行闭环
