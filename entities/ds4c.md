---
title: ds4.c 推理引擎
created: 2026-05-09
updated: 2026-05-09
type: entity
tags: [model, inference, local, apple-silicon, deepseek, antirez]
sources: [raw/articles/2026-05-08-redis-ds4c-deepseek-inference.md]
confidence: high
---

# ds4.c 推理引擎

## 概述

**ds4.c** 是Redis创始人 **antirez**（Salvatore Sanfilippo）为 **DeepSeek V4 Flash** 专门打造的本地推理引擎。核心理念：本地推理 = 推理引擎 + 专用GGUF + Agent对接验证，三件事一起做好。

**约束：Metal-only，仅支持Apple Silicon**，不支持Nvidia/AMD显卡。

## 架构

| 组成 | 占比 |
|------|------|
| C语言 | 55.4% |
| Objective-C | 30.2% |
| Metal | 13.8% |

## 模型规格

- 总参数：284B，激活参数：13B
- 上下文：100万token

## 性能

| 设备 | 量化 | 生成速度 |
|------|------|---------|
| MacBook Pro M3 Max (128GB) | 2-bit | 26.68 token/s |
| Mac Studio M3 Ultra (512GB) | - | 27.39 token/s |

## 三大关键技术

### 1. 非对称MoE量化
- 仅量化MoE专家层（up/gate: IQ2_XXS, down: Q2_K）
- 保留Q8精度的部分：共享专家层、投影层、路由层

### 2. KV缓存磁盘存储
- 传统：每次请求重新prefill
- ds4方案：KV状态写入磁盘，token前缀匹配后直接加载
- **适用场景**：Claude Code等每次启动发送25K token的Agent

### 3. API兼容层
- `/v1/chat/completions`（OpenAI协议）
- `/v1/messages`（Anthropic协议）
- 支持tool calling

已测试：opencode、Pi、Claude Code

## antirez其人

- 1977年生于西西里岛，2009年创建Redis（GitHub 7.4万Star）
- 其他项目：Kilo（<1000行C编辑器）、dump1090、linenoise
- 2022年出版科幻小说《WOHPE》
- 编程哲学：反对现代编程的复杂性堆积，崇尚简洁美感

## 参考链接

- GitHub: https://github.com/antirez/ds4
- 个人主页: http://invece.org/
