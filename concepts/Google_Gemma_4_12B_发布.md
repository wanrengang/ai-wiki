---
title: Google Gemma 4 12B 发布
date: 2026-06-04
tags:
  - AI模型
  - Google
  - 开源
  - 多模态
  - Gemma
---

# Google Gemma 4 12B 发布

## 概述

Google 发布的开源多模态 AI 模型，110亿参数，Apache 2.0 许可，可在普通笔记本（16GB VRAM）本地运行。

## 核心突破：无编码器统一架构

传统多模态模型用独立编码器处理图像/音频 → 延迟高、显存占用大。

Gemma 4 12B 彻底去掉编码器：
- 视觉编码器替换为 3500万参数的单矩阵乘法模块
- 音频编码器完全取消
- 视觉块和原始音频波形直接投影进 LLM 核心嵌入空间

## 关键能力

| 指标 | 数值 |
|------|------|
| 参数 | 11.95B |
| 上下文窗口 | 256K tokens |
| 推理模式 | 原生 "Thinking" 模式（逐步推理）|
| 函数调用 | 原生支持 |
| 音频处理上限 | 30秒 |
| 视频理解上限 | 60秒（每秒1帧） |

## 性能

- 基准测试接近 Google 更大的 26B MoE（混合专家）模型
- 支持 vLLM、SGLang、MLX、llama.cpp 等主流推理框架
- 可在 Hugging Face、Kaggle 下载

## 适用场景

- **企业数据隐私** — 本地运行敏感数据不外传
- **多模态自主Agent** — 原生函数调用+视觉+音频
- **边缘部署** — 无需持续云连接，成本低

## 局限性

- 非知识数据库，纯推理引擎（需要 RAG 补充知识检索）
- 音频/视频处理有硬上限（30秒/60秒），长内容需分块

## 相关截图

![[Google_Gemma_4_12B_发布.png]]

## 参考资料

- [Google Blog 原文](https://blog.google/innovation-and-ai/technology/developers-tools/introducing-gemma-4-12B/)
- [VentureBeat 报道](https://venturebeat.com/technology/googles-new-open-source-gemma-4-12b-analyzes-audio-video-and-runs-entirely-locally-on-a-typical-16gb-enterprise-laptop)
- [Hugging Face 模型页](https://huggingface.co/google/gemma-4-12B)
