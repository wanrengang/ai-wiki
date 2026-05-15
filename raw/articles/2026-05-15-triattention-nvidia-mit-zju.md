---
source_url: https://mp.weixin.qq.com/s/iUQyNUnphYiJAacVKwViAg
ingested: 2026-05-15
sha256: b51c67e60da8d87681a9e5d92c813d0f4a96f7d7cc305aababa34532d7cee28e
---
# 英伟达MIT出手！华人团队重磅开源，大模型推理内存暴降10倍

新智元

【新智元导读】一张普通的24G家用显卡，竟然能让一个32B的超大模型一口气读完6份长文档、自动写出周报？英伟达、MIT、浙大华人研究者联合出新招，让内存消耗直接暴降10倍，不降智也不爆显存，彻底击穿硬件天花板。

一张RTX 4090，24GB显存，跑一个32B参数的大模型做agent任务。

不做任何KV压缩，显存直接爆掉，连模型都跑不起来。

换上TriAttention，模型稳稳跑起来，顺利读完6份文档，自动生成了一份完整周报。

这不是社区大神的魔改，而是一篇来自MIT、英伟达、浙大的联合论文。

核心思路是在pre-RoPE空间里，用Q/K的三角集中度来估计每个KV token到底有多重要，然后只保留真正重要的那些。

TriAttention效果：
- 2.5倍吞吐量提升
- 10.7倍内存缩减（KV cache memory）
- AIME25数学推理任务：匹配Full Attention准确率（40.8%）

单张4090跑通32B大模型：Qwen3-32B + AWQ INT4量化 + OpenClaw agent工作流，读6份markdown文档生成周报。

vLLM插件已就位，可在现有vLLM推理管线上直接获得KV压缩收益。

作者：Weian Mao (MIT CSAIL)、Xi Lin (浙大)、Wei Huang (港大/NVIDIA Research)
论文：https://arxiv.org/abs/2604.04921