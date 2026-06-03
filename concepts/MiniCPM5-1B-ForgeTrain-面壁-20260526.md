# MiniCPM5-1B 发布：AI 编写训练框架，训出最强 1B 模型

## 核心信息

- **发布方**：面壁智能
- **发布时间**：2026年5月26日
- **模型链接**：
  - GitHub: https://github.com/OpenBMB/MiniCPM
  - Hugging Face: https://huggingface.openbmb.com/model/openbmb/MiniCPM5-1B
  - ModelScope: https://modelscope.cn/models/OpenBMB/MiniCPM5-1B

## 模型性能

- **参数量**：1B
- **AA 智能指数**：17.9 分，小尺寸模型第一
- **超越模型**：Qwen3.5-0.8B、Qwen3-0.6B、LFM2.5-1.2B-Thinking
- **击败**：Qwen3.5-2B（16.3分），参数量减半但效果更优
- **智能密度**：约每3.5个月翻一番

## 训练框架 ForgeTrain

- **全球首个完全由 AI 编写**的生产级大模型训练框架
- 使用 Claude Code 从零编写，人类零介入
- 在英伟达 H100 上比 Megatron 快 10%
- 已适配华为昇腾芯片

### 三步开发方法
1. 出考试大纲：从现有框架采集数据，定好验收标准
2. 先确保及格：AI 写出与原版结果一致的框架
3. 从及格到超越：放开限制，迭代优化直到超越原版

## 部署支持

| 精度 | 权重大小 | 适用场景 |
|------|----------|----------|
| FP16 | ~2GB | GPU、高端笔电 |
| INT8 | ~1GB | 笔电、边缘盒子 |
| INT4 | ~0.5GB | 手机、平板、车机 |

推理框架：vLLM、SGLang、llama.cpp、Ollama、LM Studio、MLX
微调框架：LLaMA-Factory、ms-swift、unsloth、xtuner、TRL+PEFT

## 开源资源

- **模型**：MiniCPM5-1B 已开源
- **数据集**：Ultra-FineWeb-L3（680B+ 英文、410B+ 中文 tokens，开源规模最大中文预训练合成数据）
- **数据治理论文**：https://arxiv.org/pdf/2602.09003
- **CPU 推理框架**：ArcLight
- **ForgeTrain**：https://github.com/OpenBMB/ForgeTrain（2026-05-26 晚上线）

## 战略意义

- **用 AI 制造 AI**：新研发范式，研发周期可从18个月压缩到6个月甚至更短
- **L3 端到端闭环交付**：ForgeTrain 是 L3 的实际落地
- **国产芯片适配**：ForgeTrain 是华为昇腾适配的第一步，解决软件生态短板

## 桌宠项目

基于 MiniCPM5-1B 的桌宠项目：https://github.com/OpenBMB/MiniPM-Desk-Pet

---

**标签**：#端侧模型 #AI训练框架 #1B模型 #MiniCPM #ForgeTrain #面壁智能 #国产适配
**来源**：AGI Hunt（微信公众号）
**日期**：2026-05-26
