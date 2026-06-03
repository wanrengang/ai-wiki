---
title: 具身智能安全综述 - Safety in Embodied AI
created: 2026-05-25
updated: 2026-05-25
type: concept
tags: [agent, safety, embodied-ai, security, survey]
sources: [raw/articles/2026-05-25-embodied-ai-safety-survey.md]
confidence: high
---

# 具身智能安全综述 - Safety in Embodied AI

> 来源：机器之心微信公众号 | 2026-05-25 | arXiv 2605.02900 | 13家机构38位学者，70+页，480+篇论文

## 核心洞察：「能力—风险」二象性（Capability-Risk Duality）

每增加一层能力，就会新增一层攻击面；能力越强，风险面也越广。

这是贯穿全文的核心组织逻辑：具身智能系统的风险正从「数字世界」逐步演化为「物理世界」。

## 五层能力圈与威胁

| 能力层 | 代表性攻击 | 真实世界后果 |
|--------|-----------|-------------|
| 感知层 | 对抗样本、后门攻击、传感器欺骗 | 障碍物漏检、停止标志误判、雷达欺骗 |
| 认知层 | 思维链劫持、推理后门 | 空间理解错误、上下文误解、错误语义推理 |
| 规划层 | 任务越狱、轨迹中毒、决策操纵 | 不安全路径规划、违反控制指令、机器人闯入禁区 |
| 行动与交互层 | 控制对抗、人机交互后门 | 机械臂撞人、车辆失控、绕过安全协议 |
| Agentic 系统层 | 工具/技能滥用、记忆投毒、记忆泄漏、级联失效 | 持久不安全行为、隐私泄漏、跨任务污染、自进化对齐崩塌 |

## 四大被低估的研究空白

1. **多模态融合的脆弱性** — 融合越多模态，安全越复杂，目前几乎没有针对融合层的攻防分析
2. **规划层在越狱攻击下的稳定性** — LLM 当 planner，越狱后果不再是「输出有害文本」，而是「机器人开始执行有害任务」
3. **开放场景下的人机交互可信度** — 传统 HRI 安全假设交互是闭合的，但真实世界里的对话是开放的
4. **Agentic 系统的级联失效路径** — 记忆、工具、技能、自进化之间如何相互污染，目前缺少形式化框架

## 与已有工作的区别

绝大多数现有综述只看其中一层（VLA 对抗鲁棒性、导航稳健性、提示注入等）。

这篇综述坚持：**必须端到端地看整个 embodied pipeline，因为攻击会跨层级联**。它把「具身智能安全」放回更大的 AI 安全图景中。

## 核心结论

> 在具身智能时代，安全应当与能力同步设计，而不是事后打补丁。

## 相关链接

- 论文：https://arxiv.org/abs/2605.02900
- GitHub：https://github.com/x-zheng16/Awesome-Embodied-AI-Safety
- 网站：https://x-zheng16.github.io/Awesome-Embodied-AI-Safety/

## 跨链接

[[agent-drift-multi-agent-systems]] | [[tool-hallucination-reasoning-trap]]