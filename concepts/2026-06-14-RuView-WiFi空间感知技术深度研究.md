# RuView: WiFi空间感知技术深度研究

## 概述

**RuView** (github.com/ruvnet/RuView) 是利用商品WiFi信号实现实时空间感知、生命体征监测的开源项目。GitHub星标73.7k，核心基于CMU的DensePose From WiFi研究。

## 信号处理流程

```
WiFi Router → 电波穿透房间 → 触及人体 → 散射
    ↓
ESP32 mesh (4-6节点) 通过TDM协议在1/6/11信道捕获CSI
    ↓
多频段融合: 3信道 × 56子载波 = 168虚拟子载波/链路
    ↓
多静态融合: N×(N-1)链路 → 注意力加权跨视角嵌入
    ↓
相干门: 接受/拒绝测量 → 稳定运行数天无需调优
    ↓
信号处理: Hampel, SpotFi, Fresnel, BVP, 频谱图
    ↓
AI骨干网 (RuVector): 注意力 + 图算法 + 压缩
    ↓
输出: 17关键点姿态 + 呼吸 + 心率 + 房间指纹
```

## 核心技术

### CSI信道状态信息
- 56子载波幅度/相位变化 → 人体"信号指纹"
- 原本用于优化无线传输质量

### WiFlow架构
- TCN时序卷积: 处理CSI时间序列
- 轴向注意力: 提取空间特征
- 输出17个COCO关键点
- 精度: 92.9% PCK@20

### AETHER自学习系统 (ADR-024)
| 组件 | 参数 | ESP32内存 |
|------|------|----------|
| Transformer骨干 | 28K | 28 KB |
| 投影头 | 25K | 25 KB |
| MicroLoRA适配器 | 1.8K | 2 KB |
| **总计** | **~55K** | **55 KB** |

## 功能规格

| 功能 | 指标 |
|------|------|
| 人体存在检测 | 82.3% accuracy |
| 呼吸监测 | 6-30 BPM |
| 心率监测 | 40-120 BPM |
| 穿墙感知 | 5米水泥墙 |
| 本地推理延迟 | <10ms |
| 边缘模块 | 65个, 5-30KB each |

## 边缘模块生态

**65个WASM边缘模块**，覆盖：
- 🏠 家庭 (sleep-quality, baby-monitor, elderly-bed-exit, hvac-presence...)
- 🔒 安防 (intrusion-alert, perimeter-breach, tailgating, loitering...)
- 🏥 医疗 (respiratory-distress, cardiac-arrhythmia, sleep-apnea, fall-detect...)
- 🛒 零售 (customer-flow, dwell-heatmap, queue-length, shelf-engagement...)
- 🏭 工业 (clean-room, confined-space, forklift-proximity, livestock-monitor...)
- 🔬 研究 (emotion-detect, gesture-language, ghost-hunter...)
- 🤖 Swarm (swarm-backup-restore, swarm-cluster-monitor, swarm-mesh-manager...)

## 隐私架构 (BFLD)

三个structural invariants:
| ID | 不变量 | 强制方式 |
|----|--------|----------|
| I1 | 原始BFI永不离开节点 | Sink marker-trait + PrivacyClass |
| I2 | 身份嵌入仅在RAM中 | 无Serialize/Clone/Copy + Drop清零 |
| I3 | 跨站点身份关联不可能 | 每站点BLAKE3密钥哈希 + 每日轮换 |

**隐私模式**: 剥离HR/BR/pose，仅传输推断状态（HIPAA/GDPR/CCPA合规）

## Home Assistant集成

**每节点21个实体**:
- 11个原始信号 (presence, person_count, heart_rate, breathing_rate, motion...)
- 10个语义状态 (someone_sleeping, possible_distress, room_active, elderly_inactivity_anomaly, meeting_in_progress, bathroom_occupied, fall_risk_elevated, bed_exit, no_movement, multi_room_transition)

**Matter桥接**: Apple Home / Google Home / Alexa / SmartThings

## 应用场景

| 类别 | 场景 |
|------|------|
| 灾难救援 | WiFi-Mat穿透废墟探测幸存者 |
| 智能家居 | 灯光/HVAC随人体位置自适应 |
| 养老监护 | 跌倒检测、长期不活动告警 |
| 医疗 | 医院床位无接触生命体征监测 |
| 零售 | 客流分析、停留热力图 |
| 安防 | 入侵检测、周界监控 |
| 工业 | 叉车接近告警、密闭空间安全 |

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | Tauri v2 |
| 后端 | Rust (wifi-densepose-sensing-server) |
| CSI运行时 | rvCSI |
| AI模型 | RuVector (Transformer + GNN) |
| 向量搜索 | HNSW (micro-hnsw-wasm) |
| 容器格式 | RVF |
| 集成 | MQTT + Matter + Claude Code插件 |

## 资源

- GitHub: https://github.com/ruvnet/RuView
- Demo: https://ruvnet.github.io/RuView
- HuggingFace: ruvnet/wifi-densepose-pretrained
- rvCSI: https://github.com/ruvnet/rvcsi
- 架构决策记录: 45个ADR

## 评价

RuView证明了"我们真的需要那么多摄像头吗"。传统方案需3-4个摄像头覆盖全家，RuView仅需1-2个WiFi收发器，无摄像头心理负担，且隐私设计从架构层面保证。

**核心优势**:
- 无图像/视频，纯信号感知
- 本地处理，不上云
- 穿墙/黑暗环境可用
- $9硬件成本
- 45个架构决策文档支撑

---

*标签: WiFi感知, CSI, 人体姿态估计, 边缘计算, 隐私计算, Home Assistant, Matter, ESP32*
*来源: GitHub RuView + 微信公众号「量子智元」2026-06-14*
