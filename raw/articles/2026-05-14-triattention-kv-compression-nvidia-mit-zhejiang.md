---
source_url: https://mp.weixin.qq.com/s/iUQyNUnphYiJAacVKwViAg
ingested: 2026-05-14
sha256: 1521ac32ae3be51da31283fbf618eb11991798a53d21e3ad9b7fe88149424706
---
# 英伟达MIT出手！华人团队重磅开源，大模型推理内存暴降10倍

新智元 新智元

### 【新智元导读】一张普通的24G家用显卡，竟然能让一个32B参数的大模型一口气读完6份长文档、自动写出周报？英伟达、MIT、浙大华人研究者联合出新招，让内存消耗直接暴降10倍。

## 核心突破

**TriAttention** 来自 MIT、英伟达、浙大联合论文，核心思路是在 pre-RoPE 空间里，用 Q/K 的三角集中度来估计每个 KV token 的重要性，然后只保留真正重要的那些。

- **10.7倍 KV cache 内存缩减**
- **2.5倍吞吐量提升**
- 在 AIME25 数学推理任务上，匹配 Full Attention 准确率（40.8%）

## 技术原理

现有 KV cache 压缩方法把每个行李都压扁（量化），TriAttention 则是先判断哪些 token 值得留、哪些可以扔——相当于"先翻一遍行李箱，把砖头扔掉，只给羽绒服打包"。

## 消费级硬件部署

单张 RTX 4090（24GB）成功运行 Qwen3-32B（AWQ INT4 量化）+ OpenClaw agent 工作流，读取 6 份 markdown 文档生成完整周报。

## 工程落地

- **vLLM 插件**：挂上就能用，不需要改模型架构或重新训练
- **Apple Silicon**：MLX 实验性支持（M1-M4）

## KV 压缩两条路线对比

| 路线 | 代表方案 | 思路 |
|------|---------|------|
| 量化派 | Google TurboQuant | 把每个行李都压扁 |
| 选择性保留派 | TriAttention | 直接减少行李数量 |

理论上两者可以叠加使用。

## 作者

- Weian Mao（MIT CSAIL 博士后）
- Xi Lin（浙江大学本科生）
- Wei Huang（香港大学博士生，NVIDIA Research 实习）

## 参考

- 论文：https://arxiv.org/abs/2604.04921
- GitHub：https://github.com/WeianMao/triattention
