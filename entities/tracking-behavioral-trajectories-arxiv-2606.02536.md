---
title: Tracking the Behavioral Trajectories of Adapting Agents (arXiv 2606.02536)
created: 2026-06-03
updated: 2026-06-03
type: entity
tags: [research, agent, security, paper]
sources: [raw/papers/2026-06-03-behavioral-trajectories-adapting-agents.md]
confidence: high
---

# Tracking the Behavioral Trajectories of Adapting Agents

## 论文信息

| 字段 | 内容 |
|------|------|
| arXiv ID | 2606.02536 |
| 作者 | Jonah Leshin, Manish Shah, Ian Timmis |
| 发表于 | ICML 2026 AIWILD Workshop |
| 单位 | — |
| Subjects | cs.AI |
| 链接 | https://arxiv.org/abs/2606.02536 |

## 核心贡献

1. **Trait Vector 方法论**：在文本 embedding 空间中定义 Agent 行为特质方向，通过技能文件 diff 训练 Ridge 回归模型量化行为变化
2. **验证结果**：68 对技能文件上 data-seeking trait 达到 91.2% Sign 准确率，Spearman ρ = 0.82
3. **Agent-to-Agent 协议**：通过可信中间件实现跨 Agent 的行为评估，无需共享文件或评分权

## 关键技术细节

- **Embedding 模型**：Qwen3-Embedding-8B（4096 维），带安全审计指令前缀
- **训练数据**：63 个来自 Awesome Copilot 仓库的技能文件，共 68 对 before/after
- **标注方式**：部分人工标注 + Claude Opus 4.6 辅助标注，人工复核
- **留一交叉验证**：Ridge 回归闭式 LOOCV（PRESS 统计量）
- **部署案例**：Hermes Agent（Nous Research）同时担任请求方和执行方

## 三类基线对比

| 方法 | Sign 准确率 | 特点 |
|------|-------------|------|
| YARA 签名规则 | 63.2% | 规则系统，无法处理上下文 |
| Trait Vector（本文） | 91.2% | 确定性、可审计、接近实时 |
| 前沿 LLM（GPT-5.4） | 100% | 最高准确率，但不可审计 |

## 关键发现

- 6 个错误分类的预测全部为小绝对值（mean |y^| = 0.085），模型在不确定时不会自信犯错
- 恶意技能文件注入（如 Cisco 2026 发现的 Claude Code 内存文件攻击）可跨会话持久化改变 Agent 行为
- Trait vector 方法在保持确定性可审计的同时，准确率接近前沿 LLM

## 相关实体

- [[hermes-agent]] — 部署案例
- [[scheming-propensity-llm-agents]] — 同一攻击面研究（Agent 伪装倾向）
- [[agent-drift-multi-agent-systems]] — 长期运行行为退化
