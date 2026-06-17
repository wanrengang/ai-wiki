---
source_url: https://github.com/openai/whisper
ingested: 2026-06-17
sha256: fc3fd13605cc287bcdfb80a80f973b21735d8e1d9220ea8665cf84155f5cd2aa
title: Whisper 项目深度调研 - OpenAI 通用语音识别模型
tags:
  - Whisper
  - ASR
  - OpenAI
  - 语音识别
  - Encoder-Decoder
  - 调研
project_path: /Users/wrg/mystudy/whisper
created: 2026-06-17
updated: 2026-06-17
category: 学习与研究
author: 万智创界
---

# Whisper 项目深度调研

> [!info] 调研对象
> **OpenAI Whisper**(官方代码仓库本地镜像):`/Users/wrg/mystudy/whisper/`
> 通用、多任务、多语种语音识别 Encoder-Decoder Transformer。
> 论文:Robust Speech Recognition via Large-Scale Weak Supervision(arXiv:2212.04356,2022-09)
> 调研日期:2026-06-17

---

## 一、项目定位

Whisper 是 OpenAI 在 2022 年发布的**通用语音基础模型**,核心定位:

- **多任务**:ASR(同语种转录)+ 语音翻译(任意语种 → 英文)+ 语种识别 + 语音活动检测(VAD)
- **多语种**:99 种语言,涵盖主流语种
- **统一架构**:所有任务**用一个模型 + 一套 token 序列约定**完成,不分模型

**与其他 ASR(Wav2Vec 2.0 / HuBERT / Conformer)的本质区别**:那些模型通常需要在大规模无标注数据上预训练,再在小数据集上微调;Whisper 走"大规模弱监督"路线 —— 用海量(68 万小时)网络音频 + 弱标注直接训练,**零样本即可用**。

---

## 二、模型架构

### 2.1 整体结构

**标准 Encoder-Decoder Transformer**:

```
音频(30s/16kHz) → log-Mel 频谱(80 或 128 mel) → AudioEncoder → Decoder → BPE token 序列
```

**关键不变量**(贯穿整个代码库,改一处需多处协调):

| 不变量 | 值 | 来源 |
|---|---|---|
| 采样率 | 16 kHz | `audio.SAMPLE_RATE` |
| 窗口长度 | 30 秒 | `audio.CHUNK_LENGTH` |
| Mel 维度 | 80(v1/v2)/ 128(v3/turbo) | 从 checkpoint 读,**不**硬编码 |
| BPE 编码 | tiktoken,多语种/英文两套 | `tokenizer.py` |

### 2.2 训练任务如何"塞"进一个模型

多任务不是分模型,是用**特殊 token** 拼出 prompt:

```
<|startoftranscript|> <|ja|> <|transcribe|> <|notimestamps|> 实际文本...
```

特殊 token 类别:
- **任务标识**:`<|transcribe|>` / `<|translate|>`
- **语种标识**:99 个语言 token(从 mel 的第一个 30s 窗口预测)
- **时间戳 token**:1500 个(30s × 50 帧/秒),用于段级 / 词级时间对齐

**decoder 一次预测**出:语种、任务、时间戳、文本 —— **4 个传统 pipeline 任务合 1**。

### 2.3 5+1 种模型规模

| Size | 参数 | English-only | Multilingual | 显存 | 速度(相对 large) |
|---|---:|---|---:|---:|---:|
| tiny | 39 M | `tiny.en` | `tiny` | ~1 GB | ~10x |
| base | 74 M | `base.en` | `base` | ~1 GB | ~7x |
| small | 244 M | `small.en` | `small` | ~2 GB | ~4x |
| medium | 769 M | `medium.en` | `medium` | ~5 GB | ~2x |
| large | 1550 M | — | `large`(= large-v3) | ~10 GB | 1x |
| **turbo** | **809 M** | — | `turbo`(= large-v3-turbo) | ~6 GB | **~8x** |

> [!tip] 选型建议
> - **英文转录**:`tiny.en` ~ `medium.en` 显著优于对应多语种版本(同尺寸下 WER 更低);`small.en` 以上差距变小
> - **速度优先(2024+)**:用 `turbo`,速度是 large 的 8x,精度损失极小
> - **翻译任务**:`turbo` 不支持,必须用 `medium` 或 `large`

---

## 三、核心代码结构(`whisper/` 包,~9 个模块)

> 这是仓库 `CLAUDE.md` 已有的精炼总结,直接引用。

| 模块 | 行数 | 职责 |
|---|---:|---|
| `__init__.py` | 7.4 KB | `load_model()`、`_MODELS` URL/SHA 注册表、`_ALIGNMENT_HEADS` 时间对齐掩码 |
| `audio.py` | 4.9 KB | ffmpeg 子进程加载音频、`log_mel_spectrogram`、30s 填充/裁剪 |
| `model.py` | 11.7 KB | `Whisper` / `AudioEncoder` / `TextDecoder` / `MultiHeadAttention` / `ResidualAttentionBlock` |
| `decoding.py` | 32.2 KB | token 级推理,`DecodingTask`、`GreedyDecoder` / `BeamSearchDecoder`、`LogitFilter`(时间戳/空白抑制/语种) |
| `transcribe.py` | 30.4 KB | **长音频转录主循环** + CLI 入口(`whisper` 脚本) |
| `tokenizer.py` | 12.3 KB | tiktoken 封装、LANGUAGES 表、特殊 token 词表 |
| `timing.py` | 12.7 KB | 词级对齐(DTW + cross-attention);带 Triton kernel + NumPy fallback |
| `triton_ops.py` | 3.6 KB | median filter / DTW 的自定义 CUDA kernel |
| `utils.py` | 11.5 KB | 输出写入器(txt/vtt/srt/tsv/json)、CLI argparse、format_timestamp |
| `normalizers/` | — | 文本归一化(BasicTextNormalizer / EnglishTextNormalizer)用于 WER 评估 |
| `assets/` | — | `mel_filters.npz`、`gpt2.tiktoken`、`multilingual.tiktoken` 二进制资源 |

### 几个值得注意的设计

1. **30 秒窗口是硬约束**:`audio.py` / `model.py` / `transcribe.py` 三处都依赖这个常量,改一处必须三处同步,**且需要重新训练 checkpoint**
2. **长音频 = 滑窗**:`transcribe.py` 滑动 30s 窗口,失败段自动升温重试(greedy fallback),`prompt` / `prefix` / `condition_on_previous_text` 串接段间上下文
3. **幻听检测**:`compression_ratio_threshold` + `logprob_threshold` 双重过滤,避免重复输出无意义文字
4. **时间对齐**:`_ALIGNMENT_HEADS` 标记每模型哪几个 cross-attention head 对齐最准,custom checkpoint 没这个就丢精度

---

## 四、使用方式

### 4.1 CLI(最常用)

```bash
# 安装
pip install -U openai-whisper
brew install ffmpeg   # 必装,运行期依赖

# 转录英文
whisper audio.flac --model turbo

# 指定语种
whisper japanese.wav --language Japanese

# 翻译到英文(只能用多语种模型,不能 turbo)
whisper japanese.wav --model medium --language Japanese --task translate
```

### 4.2 Python API(高层)

```python
import whisper
model = whisper.load_model("turbo")
result = model.transcribe("audio.mp3")
print(result["text"])
```

### 4.3 Python API(底层三步)

```python
import whisper
model = whisper.load_model("turbo")

audio = whisper.pad_or_trim(whisper.load_audio("audio.mp3"))
mel = whisper.log_mel_spectrogram(audio, n_mels=model.dims.n_mels).to(model.device)

_, probs = model.detect_language(mel)        # 1) 语种识别
print(f"Detected: {max(probs, key=probs.get)}")

result = whisper.decode(model, mel, whisper.DecodingOptions())  # 2) 解码
print(result.text)
```

---

## 五、优缺点与适用场景

### 优点

- ✅ **零样本可用**:不需要微调,99 种语言开箱即用
- ✅ **多任务合一**:转录 + 翻译 + 语种识别 + VAD 一次完成
- ✅ **多规模**:从 39M(嵌入式)到 1.5B(高精)灵活选型
- ✅ **生态成熟**:PyPI 6 年稳定维护,集成到无数下游(faster-whisper、whisper.cpp、WhisperX 等)
- ✅ **MIT 协议**:可商用
- ✅ **CLI 极简**:`whisper file.wav` 一行能用

### 缺点 / 局限

- ❌ **30 秒窗口**:长上下文靠 prompt 串接,**段间一致性不如端到端模型**
- ❌ **无标点恢复**(v1/v2):v3 / turbo 才有;老模型输出无标点
- ❌ **幻听**:无音频/纯静音段可能输出重复文字
- ❌ **不支持流式**:必须等 30s 窗口填满才能解码,延迟敏感场景(实时字幕)不直接适用
- ❌ **CPU 推理慢**:large 在 CPU 上近实时困难,需要 faster-whisper / whisper.cpp
- ❌ **训练数据不公开**:68 万小时音频的来源、版权、偏见无法审计
- ❌ **中文 WER 不顶尖**:实际项目里中文 ASR 常用 Paraformer / SenseVoice 替代

### 适用场景

| 场景 | 适合 | 备注 |
|---|---|---|
| 英文播客/会议转录 | ✅ | turbo 性价比最高 |
| 跨语种音视频字幕 | ✅ | 大模型翻译质量可用 |
| 短音频(≤30s) | ✅ | 原生窗口 |
| 长音频(>30min) | ⚠️ | 段间漂移,需后处理 |
| 中文生产 | ⚠️ | 可用,但 WER 非最优 |
| 实时流式 | ❌ | 改用 streaming-Whisper / moonshine |
| 嵌入式/边缘 | ⚠️ | 改用 whisper.cpp + tiny 模型 |

---

## 六、关键数字

- **总代码量**:~120 KB Python 源码,9 个核心模块
- **模型数量**:6 档 13 个权重(4 个 .en + 4 个多语种 + 3 个 large + 1 个 turbo + 1 个 default large=large-v3)
- **训练数据**:68 万小时弱监督音频
- **支持语种**:99 种
- **核心约束**:30s / 16kHz / 80-128 mel,不可绕过
- **协议**:MIT
- **首次发布**:2022-09;large-v3 2023-11;turbo 2024-09

---

## 七、延伸阅读(下游生态)

- **faster-whisper**:CTranslate2 后端,4x 加速,内存减半
- **whisper.cpp**:C++ 移植,可在 CPU/移动端/嵌入式运行
- **WhisperX**:加 forced alignment,长音频时间戳更准
- **insanely-fast-whisper**:基于 faster-whisper + Flash Attention 2
- **vllm-Whisper / TensorRT-LLM**:服务化部署

---

## 八、与我工作的关联

[[ai-wiki 归档工作流]] 触发判定:这是一份**开源模型架构调研**,命中 "AI/ML 主题 + 来源深度 ≥ 2(论文+代码+README)" 两个维度。如果决定归档,跑:

```bash
cd ~/coding/ai-wiki && bash _tools/check-coverage.sh "Whisper ASR OpenAI"
```

可能落到 `entities/whisper.md`(具体模型实体)。

---

💡 **万智创界 - AI技术实战派布道者**

关注我,你将获得:
- ✅ AI前沿动态与趋势
- ✅ 真实项目案例 + 代码
- ✅ 工程化实践与避坑

让 AI 真正为业务创造价值,从理论到落地,我们一起前行!
> 本文原文已同步到 GitHub,仓库地址如下:
> [https://github.com/wanrengang/wanzhi-ai-lab](https://github.com/wanrengang/wanzhi-ai-lab)
