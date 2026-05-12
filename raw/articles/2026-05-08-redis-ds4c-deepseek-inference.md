---
source_url: https://mp.weixin.qq.com/s/9X0bcfUGZYxoXuQwt89zkQ
ingested: 2026-05-09
mp_name: 量子位
---

# Redis之父为DeepSeek V4打造专用推理引擎：ds4.c

## 项目概述

**antirez**（Salvatore Sanfilippo），Redis创始人（GitHub 7.4万Star），2024年底回归Redis担任evangelist后，启动新项目ds4.c——专门为DeepSeek V4 Flash打造的本地推理引擎。

核心理念：本地推理应该是三件事一起做好——推理引擎+专用GGUF+Agent对接验证。不是拼组件，是把链路当产品设计。

## 技术架构

| 组成 | 占比 |
|------|------|
| C语言 | 55.4% |
| Objective-C | 30.2% |
| Metal | 13.8% |

**重要约束：Metal-only，仅支持Apple Silicon，不支持Nvidia/AMD显卡**

## 模型规格

- 总参数：284B，激活参数：13B
- 上下文：100万token

## 性能数据

| 设备 | 量化 | 上下文 | 预填充速度 | 生成速度 |
|------|------|--------|-----------|---------|
| MacBook Pro M3 Max (128GB) | 2-bit | 32K | 58.52 token/s | 26.68 token/s |
| Mac Studio M3 Ultra (512GB) | - | 11709 token | 468.03 token/s | 27.39 token/s |

## 三大关键技术

### 1. 非对称MoE量化
- 仅量化MoE专家层（up/gate: IQ2_XXS, down: Q2_K）
- 保留Q8精度的部分：共享专家层、投影层、路由层

### 2. KV缓存磁盘存储
- 传统方法每次请求重新做prefill
- ds4方案：KV状态写入磁盘，token前缀匹配后直接从磁盘加载
- 适用场景：Claude Code等每次启动发送25K token初始prompt的agent

### 3. API兼容层
- `/v1/chat/completions` — OpenAI协议
- `/v1/messages` — Anthropic协议
- 支持tool calling

已测试客户端：opencode、Pi、Claude Code

## 关于antirez

- 1977年出生于西西里岛，2009年创建Redis
- 其他项目：Kilo（<1000行C的编辑器）、dump1090（ADS-B信号解码器）、linenoise
- 2022年出版科幻小说《WOHPE》——主题：AI、气候变化、程序员与技术的互动
- 编程哲学：「现代编程正变得复杂、无趣，全是要粘合的层。它正失去大部分美感。」

## 参考链接

- ds4项目GitHub：https://github.com/antirez/ds4
- antirez个人主页：http://invece.org/
- Hacker News：https://news.ycombinator.com/item?id=48050751
