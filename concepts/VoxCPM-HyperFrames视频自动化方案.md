# VoxCPM + HyperFrames AI 新闻视频自动化方案

> 建立时间：2026-06-07
> 状态：方案阶段，待实施

---

## 一、项目概述

**目标**：实现 AI 新闻视频的全自动化制作，从资讯采集到成片渲染一条龙完成。

**核心技术栈**：

| 环节 | 技术方案 | 说明 |
|------|---------|------|
| 资讯采集 | AI HOT / WeRss | 自动抓取 AI 行业热点 |
| 脚本生成 | VoxCPM / 本地 LLM | 生成视频旁白文案 |
| 语音合成 | VoxCPM TTS | 本地生成专业中文旁白 |
| 视频渲染 | HyperFrames | HTML + GSAP 动画合成 MP4 |
| 质量检测 | FFmpeg | 黑屏检测、音画同步验证 |

---

## 二、两种运行模式

### 模式 A：本地运行（高配机器）

适用于有足够显存的机器（如 RTX 4090 24GB）。

**VoxCPM 模型规格**：

| 模型 | 参数量 | 显存要求 | 适用场景 |
|------|--------|---------|---------|
| VoxCPM2 | 2B | ~12GB+ | 最高质量 |
| VoxCPM1.5 | 0.5B | ~6GB | RTX 4090 推荐 |

**工作流程**：
```
资讯采集 → 脚本生成 → VoxCPM TTS → 测量音频时长 → 分配场景时间 → HyperFrames 渲染 → QA 检测
```

### 模式 B：远程调用（当前方案）

笔记本性能不足，调用台式机（RTX 4090）的 VoxCPM API。

**架构图**：
```
笔记本 (发起请求)
    │
    │  HTTP POST /tts
    ▼
台式机 (RTX 4090)
    │
    ▼
VoxCPM TTS API 服务 (:8080)
    │
    ▼
返回 WAV 音频
    │
    ▼
笔记本 (接收音频)
    │
    ▼
HyperFrames 渲染 → 成品 MP4
```

---

## 三、远程调用方案详情

### 3.1 台式机端（服务端）

**依赖安装**：
```bash
pip3 install flask scipy numpy
```

**启动服务**：
```bash
python3 voxcpm_server.py --preload --port 8080
```

**服务脚本**：`~/.openclaw/workspace/skills/voxcpm-video-maker/scripts/voxcpm_server.py`

**健康检查**：
- `GET /health` - 返回服务状态和模型加载情况
- `POST /tts` - 生成语音，返回 WAV 文件

**调用示例**：
```bash
curl -X POST http://台式机IP:8080/tts \
     -H "Content-Type: application/json" \
     -d '{"text":"要转换的文本"}' \
     -o output.wav
```

```python
import requests

resp = requests.post("http://台式机IP:8080/tts", 
                    json={"text": "要转换的文本"})
with open("output.wav", "wb") as f:
    f.write(resp.content)
```

### 3.2 笔记本端（调用方）

无需安装 VoxCPM，只需能访问台式机的 HTTP 服务即可。

**调用脚本**：`~/.openclaw/workspace/skills/voxcpm-video-maker/scripts/generate_voice_remote.py`

---

## 四、OpenClaw Skill 结构

**位置**：`~/.openclaw/workspace/skills/voxcpm-video-maker/`

```
voxcpm-video-maker/
├── SKILL.md                         # 技能说明文档
├── scripts/
│   ├── install_deps.sh              # 依赖安装脚本
│   ├── fetch_news.py                # 资讯采集脚本
│   ├── generate_voice.py            # 本地 VoxCPM TTS
│   ├── generate_voice_remote.py     # 远程 VoxCPM TTS
│   ├── voxcpm_server.py             # API 服务端（台式机用）
│   └── make_video.sh                # 主流程自动化脚本
└── templates/
    ├── index.html.template          # HyperFrames 主模板
    └── compositions/
        ├── intro.html               # 开头场景
        ├── news-item.html           # 新闻内容场景
        └── outro.html               # 结尾场景
```

---

## 五、视觉风格

支持五种视觉路线，每天自动切换：

| 路线 | 视觉特征 | 适用场景 |
|------|---------|---------|
| Signal Radar | 雷达扫描、环形网格、青色点缀 | 突发 AI 新闻 |
| Deep Space | 星空、视差滚动、缓慢宇宙运动 | 模型发布/研究 |
| Command Center | 终端代码块、数据面板、仪表盘 | 工具/安全/商业更新 |
| Magazine Motion | 编辑级排版、干净卡片、大引号 | 日常新闻回顾 |
| Neon Dataflow | 流光线条、粒子、图表、流动路径 | 技术/工具故事 |

---

## 六、参考资料

- **voxcpm-video-maker 项目**（Trae SOLO）：https://github.com/rfdiosuao/voxcpm-video-maker
- **VoxCPM 官方**：https://github.com/OpenBMB/VoxCPM
- **HyperFrames 官方**：https://www.hyperframes.net/

---

## 七、后续任务清单

- [ ] 台式机安装 VoxCPM 和依赖
- [ ] 配置 voxcpm_server.py 开机自启动
- [ ] 确认台式机局域网 IP
- [ ] 笔记本端测试远程调用
- [ ] 完整流程联调测试
- [ ] 上线自动化每日视频生成

---

## 八、注意事项

1. **VoxCPM 模型下载**：首次运行会从 HuggingFace 下载模型权重
2. **音画同步**：基于实际音频时长动态分配场景时间，误差 < 0.2 秒
3. **渲染质量**：`draft` 快速迭代，`standard` 评审，`high` 最终交付
4. **网络要求**：笔记本和台式机需在同一局域网
