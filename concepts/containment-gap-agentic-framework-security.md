---
title: The Containment Gap：Agentic AI 框架结构安全缺口
created: 2026-06-14
updated: 2026-06-14
type: concept
tags: [agent, security, alignment, engineering, research]
sources: [raw/papers/2026-06-14-containment-gap-agentic-ai-framework-security.md]
confidence: high
---

# The Containment Gap：Agentic AI 框架结构安全缺口

## 概述

2026 年 arXiv 论文首次系统审计了三大主流 Agent 框架（LangChain、AutoGPT、OpenAI Agents SDK）的结构安全保证。核心问题：**框架是否在架构层面提供结构性安全边界？**

答案是否定的。论文提出**六项Containment Principles**（ containment principles）并对三大框架进行合规审计，发现零原生合规。

## 六项 Containment Principles

| 原则 | 编号 | 内容 |
|------|------|------|
| Reasoning-Execution Separation | P1 | 策略门（Policy Gate）位于规划与执行之间，Agent 不能执行其构思的所有计划 |
| Capability Scoping | P2 | 每个会话有受限 Token，明确可用工具、参数范围、速率限制和过期时间 |
| Memory Integrity | P3 | 任何写入长期记忆的内容须通过完整性函数验证，不通过则丢弃 |
| Layer-Transition Validation | P4 | 在数据经过的所有接口（P→B、B→E、E→U）执行安全检查 |
| Authenticated Communication | P5 | Agent 间通信须包含可验证的数字签名凭证 |
| Runtime Monitoring | P6 | 异常检测器监控执行路径，检测到异常时激活 containment |

**Theorem 1（Containment Sufficiency）：** 如果系统同时满足 P1（策略门控执行）和 P3（内存写验证），则单步记忆污染攻击无法造成持久策略违规。

## 三大框架合规矩阵

| 原则 | LangChain | AutoGPT | OpenAI SDK |
|------|-----------|---------|------------|
| P1 | ✓*（需显式配置 Callbacks） | ✗（直接透传） | ✓*（Guardrails，可选） |
| P2 | ✓* | ✗ | ✓* |
| **P3 Memory Integrity** | **✗** | **✗** | **✗** |
| P4 | ✓* | ✗ | ✓* |
| P5 | ✗ | ✗ | ✓* |
| P6 | ✓* | ✓* | ✓* |

**关键发现：**
1. **零原生合规** — 没有任何框架在任何原则上达到 ✓（原生默认启用）
2. **P3 全面失败** — 三大框架均无内存完整性保护
3. **安全是可选的** — 所有保护机制均需显式配置，违反 Secure-by-Default 原则
4. **自主性反比合规** — AutoGPT 最激进（最自治），合规性最差（5 项不合规）

## 实证攻击结果

### 实验设置
LangChain 福利申请 Agent（250 条合成申请），Qwen-2.5 3B-Instruct + Claude Haiku 4.5 + GPT-4o 五种后端。

### 记忆污染攻击（Exp. 1）
在第 11/40 条申请时注入单条恶意记忆（伪造策略：Region B 收入<$30k 应拒绝）：
- **腐败率：1.000（全部三个 seed）**
- 平均准确率从 0.908 跌至 0.558（35 个百分点下降）
- **目标群体（Region B）错误拒绝率：88.9%**

### 记忆完整性验证器（Exp. 2）
确定性正则表达式验证器（检查来源溯源、Schema 合规、人口统计瞄定性），无 LLM 调用：
- 腐败率从 **1.000 降至 0.000**（所有 seed）
- 准确率恢复至 0.967
- **开销：0.016ms/次**

### 工具策略绕过（Exp. 3）
100 条恶意工具调用（路径穿越、未授权 API、限制写入），无门控时：
- **绕过率：100%**
- 策略门（deny-all allowlist + 路径规范化）后：
- **绕过率：0.000，开销：0.129ms/次**

### 跨模型泛化（Exp. 3）
即使 Claude Haiku 4.5 和 GPT-4o（已对齐），记忆污染腐败率仍为 **1.000** — 因为对齐发生在下游，而攻击靶向上游记忆。

### 复杂策略隐蔽性（Exp. 4）
五因子conjunctive 策略（收入、户口、抚养人、先前福利、居住年限）：
- 总体准确率：几乎不变（掩盖伤害）
- Region B 错误拒绝：上升 **3–3.5×**
- 验证器将腐败率从 1.000 降至 0.000（Claude）/ 0.167（GPT-4o-mini）

## 三项优先干预建议

1. **P2：工具声名为能力边界** — 每次会话必须声明工具列表，默认 deny-all，按会话允许工具
2. **P1：推理与执行间强制策略门** — 推理输出在进入工具执行前必须经过策略门
3. **P3：溯源验证的内存写入** — 内存写入须包含 provenance，存储前验证，默认拒绝不可信输入

## 关键洞察

- **安全是架构问题，不是模型问题**：即使对齐后的 RLHF 模型（Claude Haiku/GPT-4o），在框架层面缺乏结构性边界时同样完全脆弱
- **记忆污染是结构性漏洞**：不是某框架的实现 bug，而是组合架构缺少边界隔离的结构性缺陷
- **聚合指标掩盖定向伤害**：复杂策略下总体准确率稳定，而目标群体受害率上升 3.5×，标准监控无法察觉
- **轻量级确定性方案有效**：验证器和策略门均无需 LLM，<1ms 开销，可直接包装现有框架抽象

## 相关概念

- [[harness-engineering]] — PE→CE→HE 演进框架，Agent 约束与控制
- [[mcp]] — Model Context Protocol，工具调用标准化
- [[lcguard-kv-latent-communication]] — KV 潜在通信安全，对抗信息泄露
- [[agent-drift-multi-agent-systems]] — 多智能体长期行为退化
- [[metr-frontier-risk-report]] — METR 前沿风险报告，AI 欺骗行为
- [[scheming-propensity-llm-agents]] — LLM Agent 伪装倾向，工具访问是安全杠杆
