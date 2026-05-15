---
source_url: https://mp.weixin.qq.com/s/v9fNeL-gHM54HhE6pmk7Yg
ingested: 2026-05-13
sha256: 7c7ed5bc96866d4c0230d0f82fd611a740bf9e0a29f3bdd7d84db3366b1826e7
---

# ICML 2026 | APO: 多模态大模型推理对齐

## Overview

悉尼科技大学（UTS）研究团队提出 **APO（Autonomous Preference Optimization）** — 突破传统蒸馏对单一强教师模型的依赖，通过多流教师模型的协同机制，将"概念漂移"转化为动态负约束，实现稳健的多模态大模型推理对齐。被 ICML 2026 接收。

**核心洞察**：多源 MLLM 的真实推理流形潜藏在多流共识之中，而非单一强教师监督。单纯模仿漂移的教师流会内化各模型偏见，导致幻觉与语义不一致。

---

## 核心方法

### 问题定义：非平稳多流概念对齐

研究团队将框架扩展至多源设定：对于包含 N 个独立推理流的环境，推理步骤 j 时的集体状态为各源模型独立状态的联合分布。当联合分布随推理步骤呈现非平稳演化，即发生**多流推理漂移**。

### 两阶段协议

**第一阶段：监督引导的共识合成**
- 学生模型广泛吸收所有教师模型的异构知识
- 建立包容集体智慧的基础能力基座
- 利用大模型推理能力进行上下文共识提取
- 自主过滤缺乏跨模型支持的矛盾信息，放大逻辑交集

**第二阶段：约束感知的偏好优化**
- 共识轨迹作为正向引导信号
- 教师模型相互冲突的推理轨迹重构为动态负约束
- 扩展 DPO（Direct Preference Optimization），将共识轨迹与一整套漂移输出进行偏好优化
- 最大化共识与漂移之间的边际，有效抑制推理空间中的漂移模式

---

## 数据集：CXR-MAX

专为高风险领域多教师蒸馏研究设计的大规模基准：
- 扩展自 MIMIC-CXR 数据集
- 汇集 7 个主流 MLLM 的推理轨迹：GPT-5, Gemini-2.5, Sonnet-4, Grok-4, Qwen-VL-MAX, GLM-4.5V, Moonshot
- 170,982 个推理实例，涵盖 14 种胸部疾病

---

## 实验结果

APO 训练的 7B 模型在所有胸片疾病诊断任务中实现 **0.78 平均准确率**，超越包括 GPT-5 在内的所有教师模型。

特别在实变（Con.）、水肿（Ede.）等疾病预测中，教师模型间准确率落差超过 70%，APO 学生模型在几乎所有类别中都稳居前二，展现极强稳定性。

---

## 论文信息

- 论文：Turning Drift into Constraint: Robust Reasoning Alignment in Non-Stationary Multi-Stream Environments
- 作者：Xiaoyu Yang, En Yu, Wei Duan, Jie Lu（悉尼科技大学 AAII）
- 链接：https://arxiv.org/abs/2510.04142
- 项目主页：https://xiaoyuyoung.github.io/APO/
- 代码：https://github.com/XiaoyuYoung/APO
