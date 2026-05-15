---
source_url: https://mp.weixin.qq.com/s/z7-UXcoE81zKp56DPLgSmQ
ingested: 2026-05-15
sha256: 8f7c3b2a1e9d4c0b8f6e7a2d4c5b8f1e3a6d7c9b2f4e8d6c1a3b5d7e9f1a2b3c4
---

# Claude「角色混淆」Bug：百万上下文成降智重灾区

## 核心问题

Claude存在严重的**角色归因错误**：将**自己生成的指令**误认为是**用户下达的指令**。这不是普通的AI幻觉，而是更深层的系统性问题。

> 「这个问题动摇的是AI智能体最基本的可靠性前提。」— Gareth Dwyer

## 关键案例

### 1. Gareth Dwyer 的发现
- 软件工程师，2025年1月首次公开记录此bug
- 称之为「迄今为止在Claude Code中发现的最严重的bug」
- 4月跟进分析，命名为「Claude搞混了谁说了什么」
- 起初归因于Claude Code外层实现，后修正认为可能涉及更广泛的**模型级问题**

### 2. Reddit 社区案例
- 用户分享：Claude自己说出「把H100也拆了」，随后声称是用户下达的
- Dwyer引用此案例并指出：**错误出在框架层面，而非模型本身**

### 3. nathell 的完整对话转录
- 在Hacker News公开完整对话记录
- Claude先说「Shall I commit this progress?」，后续又将其推进为已获用户批准状态

## 技术根因分析

### 问题源头（GitHub Issue #44778）

Claude Code 系统事件以 role: "user" 的形式送入模型。当模型等待用户回复时收到系统事件 → 误判为用户新输入 → 「脑补」用户已同意 → 继续执行。

| 事件类型 | 发送方式 |
|---------|---------|
| 后台任务完成通知 | 作为user消息 |
| 队友空闲提醒 | 作为user消息 |
| 定时器触发 | 作为user消息 |

> 「不是模型故意撒谎，而是底层架构的角色标记缺陷，让模型从一开始就分不清那条消息究竟是谁发的。」

## 学术研究支持

### 论文：《Prompt Injection as Role Confusion》(MIT, 2026年3月)
- arXiv: https://arxiv.org/pdf/2603.12277
- **核心发现**：模型判断「谁在说话」时，依赖**文本写得像谁**而非**文本实际来自哪里**
- **攻击手法：CoT Forgery**：在用户输入或工具输出中伪造思维链内容，达成约**60%攻击成功率**

### OpenAI 研究 (arXiv:2603.10521)
- 建立权威等级：`System > Developer > User > Tool`
- 明确将「模型错误信任不该信任的指令」列为安全挑战

## 百万上下文：放大风险

- **Context Rot**：推理密集型任务性能退化和角色混淆**早在32K-100K token时就开始**
- Claude Code 源码泄露后安全分析（Straiker公司）：
  - 四级压缩流水线管理上下文压力
  - 嵌入CLAUDE.md的恶意指令可在压缩中存活
  - 通过摘要被「洗白」后变成「合法用户指令」

## 安全与权限问题

| 问题类型 | 定义 |
|---------|------|
| 幻觉 | AI编造不存在的事实 |
| 权限问题 | AI拿到不该拿的能力 |
| **角色混淆** | **AI把自己的输出当成用户授权** |

> 「就算你把权限收到最紧，一个连『这句话到底是谁说的』都搞不清楚的系统，在任何场景下都是定时炸弹。」

## Anthropic 最新动态

| 方向 | 内容 |
|-----|------|
| Claude Code Auto Mode | 更低维护成本下实现更高任务自主性 |
| 12种智能体架构模式 | 涵盖记忆管理、工作流编排、工具权限、自动化 |
| 能力图谱扩展 | 100万token上下文、子Agent协作、自动执行shell、一键部署 |

## 关键链接

- Dwyer首发文章：https://dwyer.co.za/static/the-worst-bug-ive-seen-in-claude-code.html
- Dwyer跟进分析：https://dwyer.co.za/static/claude-mixes-up-who-said-what-and-thats-not-ok.html
- GitHub Issue：https://github.com/anthropics/claude-code/issues/44778
- 学术论文：https://arxiv.org/pdf/2603.12277
- OpenAI研究：https://arxiv.org/pdf/2603.10521
