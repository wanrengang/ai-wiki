---
source_url: https://arxiv.org/abs/2511.12271
ingested: 2026-05-20
sha256: 57bfb16d851b2a28b0ed1e145784634f32474d8317df56017174ce6808906f0d
---
# MoralReason: Generalizable Moral Decision Alignment For LLM Agents Using Reasoning-Level Reinforcement Learning

## 论文信息
- arXiv: 2511.12271v1
- 会议：AAAI 2026
- 主题：推理级强化学习（Reasoning-Level RL）对齐 LLM Agent 道德决策

## 训练方法

### SFT 流程
- 自定义数据集：三种道德框架（功利主义、义务论、美德伦理）各取前20%训练集
- 训练集与评估集完全隔离，防止数据泄露

### 数据集划分
| 集合 | 数量 | 用途 |
|------|------|------|
| 过滤后分歧场景 | 171 | 无全票一致的行动对齐/反对 |
| GRPO训练（前70%） | 119 场景 | OOD对齐训练 |
| 评估（后50） | 50 场景 | 测试 |

### 训练配置
```yaml
Learning rate: 5×10⁻⁶ (linear decay)
Batch size: 1 per device
Generations per step: K=4
Maximum training steps: 150
Temperature (training): 1.0
Temperature (evaluation): 0.1
Optimizer: AdamW (weight decay: 0.01)
```

## 实验结果

### OOD Alignment Score

| Model | Utilitarian | Deontological | Virtue Ethics | Change |
|-------|-------------|---------------|----------------|--------|
| Base | 0.207 | 0.207 | 0.586 | — |
| Utilitarian-aligned | 0.964 | 0.014 | 0.022 | +0.757 |
| Deontological-aligned | 0.265 | 0.657 | 0.079 | +0.450 |
| Virtue-aligned | — | — | — | No improvement |

### 关键发现

1. **Base模型偏见**：Qwen3-4B-Base 强烈偏好美德伦理（0.586），而非功利/义务论（均为0.207）

2. **功利主义对齐最成功**：达到 0.964 OOD分数（+0.757绝对提升），排他性地聚焦功利推理，有效压制其他框架

3. **义务论对齐次之**：在step 75达到峰值0.657，之后出现退化，暗示过拟合/不稳定

4. **美德伦理对齐无效**：Base模型已有美德偏见，GRPO对齐无法进一步改进

### 训练曲线观察
> GRPO训练过程中所有三种模型的alignment reward都显示出显著方差——原因是采样响应时的温度设置较高，鼓励模型探索不同响应。

## 未来方向

1. 扩展框架覆盖：纳入非西方道德框架
2. 改进reward公式化：结构化道德目标表示、语义感知度量、自适应reward加权
3. 人类反馈集成：交互学习、偏好比较、参与式标注