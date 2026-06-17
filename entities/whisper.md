---
title: Whisper
created: 2026-06-17
updated: 2026-06-17
type: entity
tags:
  - model
  - multimodal
  - openai
  - asr
sources:
  - raw/articles/2026-06-17-whisper-deep-research.md
confidence: high
---

# Whisper

## 概述

OpenAI 在 2022-09 发布的通用语音识别模型,论文 [arXiv:2212.04356](https://arxiv.org/abs/2212.04356)(Robust Speech Recognition via Large-Scale Weak Supervision)。走"大规模弱监督"路线,用 68 万小时网络音频 + 弱标注直接训练,**零样本可用**,无需微调。

核心差异化:与其他 ASR(Wav2Vec 2.0 / HuBERT / Conformer)相比,Whisper 不需要"预训练 + 微调"两段式,而是一个端到端的统一模型。

## 关键事实

| 维度 | 数据 |
|---|---|
| 首次发布 | 2022-09 |
| large-v2 | 2022-12 |
| large-v3 | 2023-11 |
| turbo(large-v3 优化版) | 2024-09 |
| 训练数据 | 68 万小时弱监督音频 |
| 支持语种 | 99 种 |
| 核心约束 | 30s / 16kHz / 80-128 mel,不可绕过 |
| 协议 | MIT |
| 模型数量 | 6 档 13 个权重 |
| 参数量范围 | 39M(tiny)~ 1550M(large) |

## 模型规格

| Size | 参数 | English-only | Multilingual | 显存 | 速度(相对 large) |
|---|---:|---|---:|---:|---:|
| tiny | 39 M | `tiny.en` | `tiny` | ~1 GB | ~10x |
| base | 74 M | `base.en` | `base` | ~1 GB | ~7x |
| small | 244 M | `small.en` | `small` | ~2 GB | ~4x |
| medium | 769 M | `medium.en` | `medium` | ~5 GB | ~2x |
| large | 1550 M | — | `large`(= large-v3) | ~10 GB | 1x |
| **turbo** | 809 M | — | `turbo` | ~6 GB | **~8x** |

> **选型提示**:英文转录用 `.en` 版本精度更高;**2024+ 速度优先用 turbo**(精度损失极小);翻译任务 turbo 不支持,必须用 medium / large。

## 架构

**标准 Encoder-Decoder Transformer**,输入 log-Mel 频谱,输出 BPE token 序列。多任务不用分模型,用**特殊 token** 拼 prompt:

```
<|startoftranscript|> <|ja|> <|transcribe|> <|notimestamps|> 实际文本...
```

- 任务标识:`<|transcribe|>` / `<|translate|>`
- 语种标识:99 个语言 token(从 mel 第一个 30s 窗口预测)
- 时间戳 token:1500 个(30s × 50 帧/秒),用于段级/词级对齐

**decoder 一次预测**:语种 + 任务 + 时间戳 + 文本 —— 传统 4 段 pipeline 合一。

## 核心代码结构

仓库:`/Users/wrg/mystudy/whisper/`(CLAUDE.md 已详尽描述)

| 模块 | 行数 | 职责 |
|---|---:|---|
| `__init__.py` | 7.4 KB | `load_model()`、`_MODELS` URL/SHA 注册表、`_ALIGNMENT_HEADS` 时间对齐掩码 |
| `audio.py` | 4.9 KB | ffmpeg 子进程加载音频、`log_mel_spectrogram`、30s 填充/裁剪 |
| `model.py` | 11.7 KB | `Whisper` / `AudioEncoder` / `TextDecoder` / `MultiHeadAttention` |
| `decoding.py` | 32.2 KB | token 级推理,`DecodingTask`、`GreedyDecoder` / `BeamSearchDecoder`、`LogitFilter` |
| `transcribe.py` | 30.4 KB | 长音频转录主循环 + CLI 入口 |
| `tokenizer.py` | 12.3 KB | tiktoken 封装、LANGUAGES 表、特殊 token 词表 |
| `timing.py` | 12.7 KB | 词级对齐(DTW + cross-attention);带 Triton kernel + NumPy fallback |
| `triton_ops.py` | 3.6 KB | median filter / DTW 自定义 CUDA kernel |
| `utils.py` | 11.5 KB | 输出写入器(txt/vtt/srt/tsv/json)、CLI argparse |
| `normalizers/` | — | 文本归一化(BasicTextNormalizer / EnglishTextNormalizer)用于 WER 评估 |

### 设计要点

1. **30 秒窗口是硬约束** — `audio.py` / `model.py` / `transcribe.py` 三处依赖,改一处必须三处同步 + 重新训练 checkpoint
2. **长音频 = 滑窗** — `transcribe.py` 滑动 30s 窗口,失败段自动升温重试,`prompt` / `prefix` / `condition_on_previous_text` 串接段间上下文
3. **幻听检测** — `compression_ratio_threshold` + `logprob_threshold` 双重过滤
4. **时间对齐** — `_ALIGNMENT_HEADS` 标记每模型哪些 cross-attention head 对齐最准,custom checkpoint 没这个就丢精度

## 使用

### CLI

```bash
# 安装
pip install -U openai-whisper
brew install ffmpeg

# 转录英文
whisper audio.flac --model turbo

# 指定语种
whisper japanese.wav --language Japanese

# 翻译到英文(只能用多语种模型)
whisper japanese.wav --model medium --language Japanese --task translate
```

### Python(高层)

```python
import whisper
model = whisper.load_model("turbo")
result = model.transcribe("audio.mp3")
print(result["text"])
```

### Python(底层三步)

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

## 优缺点

### 优点

- ✅ 零样本可用,99 种语言开箱即用
- ✅ 多任务合一:转录 + 翻译 + 语种识别 + VAD
- ✅ 多规模:39M(嵌入式)~ 1.5B(高精)灵活选型
- ✅ 生态成熟:6 年维护,集成到无数下游
- ✅ MIT 协议可商用
- ✅ CLI 极简

### 缺点

- ❌ 30 秒窗口:长上下文靠 prompt 串接,段间一致性不如端到端模型
- ❌ 无标点恢复(v1/v2):v3/turbo 才有
- ❌ 幻听:无音频/纯静音段可能输出重复文字
- ❌ 不支持流式:必须等 30s 窗口填满
- ❌ CPU 推理慢:large 在 CPU 上近实时困难,需要 faster-whisper / whisper.cpp
- ❌ 训练数据不公开:68 万小时音频的来源、版权、偏见无法审计
- ❌ 中文 WER 不顶尖:中文生产常用 Paraformer / SenseVoice 替代

## 适用场景

| 场景 | 适合 | 备注 |
|---|---|---|
| 英文播客/会议转录 | ✅ | turbo 性价比最高 |
| 跨语种音视频字幕 | ✅ | 大模型翻译质量可用 |
| 短音频(≤30s) | ✅ | 原生窗口 |
| 长音频(>30min) | ⚠️ | 段间漂移,需后处理 |
| 中文生产 | ⚠️ | 可用,但 WER 非最优 |
| 实时流式 | ❌ | 改用 streaming-Whisper / moonshine |
| 嵌入式/边缘 | ⚠️ | 改用 whisper.cpp + tiny |

## 下游生态

- **faster-whisper**:CTranslate2 后端,4x 加速,内存减半
- **whisper.cpp**:C++ 移植,可在 CPU/移动端/嵌入式运行
- **WhisperX**:加 forced alignment,长音频时间戳更准
- **insanely-fast-whisper**:基于 faster-whisper + Flash Attention 2
- **vllm-Whisper / TensorRT-LLM**:服务化部署

## 相关

- [[openai-deployment-company-2026-05-15]] - OpenAI 企业部署公司(OpenAI 周边生态)
- [[openai-frontier]] - OpenAI 企业级 AI 代理平台(同类企业产品对比)
- 来源:[raw/articles/2026-06-17-whisper-deep-research.md](raw/articles/2026-06-17-whisper-deep-research.md)