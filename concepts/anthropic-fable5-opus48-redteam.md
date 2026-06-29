---
title: Anthropic Fable 5 & Opus 4.8 红队对抗评估
created: 2026-06-18
updated: 2026-06-18
type: concept
tags: [alignment, security, anthropic, benchmark, agent]
sources: [raw/papers/2026-06-18-anthropic-fable5-opus48-redteam-2606.18193.md]
confidence: high
---

## 概述

**A Red-Team Study of Anthropic Fable 5 & Opus 4.8**（AI4I，Dr. Nicola Franco，2026年6月）是针对 Anthropic 前沿模型 Opus 4.8 和 Fable 5 的系统性红队对抗评估。使用 **HackAgent** 自动红队框架，在 **7,826 个有害意图**、**10 大伤害类别**上生成数十万次对抗攻击尝试，每个成功案例经由**三人法官小组**（Qwen3.7 Max、Gemini 3.5 Flash、GPT 5.5）多数投票确认为真正的越狱。

核心结论：**即使是经过最充分测试的前沿模型，在持续自动压力下依然可以被可靠地突破。**

## 攻击方法论

**四种攻击家族：**

| 攻击家族 | 类型 | 描述 |
|----------|------|------|
| **TAP**（Tree of Attacks with Pruning） | 自适应树搜索 | 攻击者模型扩展候选 prompt 树，引导评分剪枝，深度3/广度4/分支因子3 |
| **PAIR**（Prompt Automatic Iterative Refinement） | 自适应迭代 | 攻击者读取目标拒绝并重写 prompt，最多 12 步迭代，8 个并行流 |
| **PAP**（Persuasive Adversarial Prompts） | 静态劝说 | 单次修辞性重框（权威、角色扮演、假设），无目标反馈 |
| **h4rm3l** | 静态混淆 | 固定字符串转换：base64、字符密码、payload 分拆、few-shot、DAN 风格角色扮演、维基百科框架 |

**两阶段裁审：**
1. 环内评分（HarmBench 风格 rubric）——仅用于引导搜索
2. 三法官独立复审——仅当多数（2/3）判定为有害时才计为确认越狱

## 核心数据

**各技术面板确认 ASR：**

| 技术 | Opus 4.8 | Fable 5 |
|------|----------|---------|
| TAP（树搜索） | **11.51%** | 6.10% |
| PAIR（迭代精炼） | 7.98% | 4.30% |
| PAP（劝说重框） | 3.67% | 0.54% |
| h4rm3l（静态混淆） | **0.18%** | **0.04%** |

**确认有害完成总数：**
- Opus 4.8：**1,620** 个
- Fable 5：**702** 个
- **覆盖全部 10 大伤害类别（A-J）**

## 最薄弱环节

**Opus 4.8 最弱类别：**
- 儿童安全：ASR **27.6%**（TAP）
- 网络安全：16.6%（PAIR）
- 内容/文化：13.2%（TAP）
- 犯罪/经济：14.7%（TAP）

**Fable 5 最弱类别：**
- 伦理/社会：ASR 10.2%（TAP）
- 儿童安全：次高

## 关键洞察

1. **自适应攻击主导残余表面：** 95%（Opus）和 97%（Fable）的确认越狱来自自适应/劝说攻击家族；TAP 几乎总在 1-2 步内成功，后续迭代增加很小
2. **静态混淆基本无效：** h4rm3l 在约 50,000 次尝试下仅确认 85（Opus）和 21（Fable）——安全训练已有效中和固定模式
3. **绝对数量不容忽视：** 在日均百万次交互的部署规模下，6-11% 的 ASR 是稳定可复现的有害输出流
4. **"可解决" ≠ "已解决"：** 最薄弱环节具体且可定位，但尚未被修复

## 与现有研究的关联

[[containment-gap-agentic-framework-security]] — 框架级合规问题，LangChain/AutoGPT/OpenAI SDK 零原生合规  
[[tool-hallucination-reasoning-trap]] — 推理增强放大工具幻觉，ACL 2026  
[[alignment-tampering-rlhf]] — RLHF 对齐相关，h4rm3l 静态混淆被中和说明安全训练有效，但自适应攻击仍需新解  
[[agent-drift-multi-agent-systems]] — 长期运行行为退化，攻击面持续存在的另一维度
