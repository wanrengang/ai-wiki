# Unlimited-OCR 百度开源 OCR 工具研究

> 研究日期：2026-06-28
> 来源：GitHub / baidu/Unlimited-OCR

## 基本信息

| 指标 | 数据 |
|------|------|
| **官方仓库** | https://github.com/baidu/Unlimited-OCR |
| **Stars** | 11,168 |
| **Forks** | 861 |
| **License** | MIT |
| **发布于** | 2026-06-18（约10天前） |
| **语言** | Python |
| **机构** | 百度 |

---

## 核心定位

**Unlimited OCR Works: Welcome the Era of One-shot Long-horizon Parsing**

百度在 DeepSeek-OCR 基础上进一步优化的 OCR 项目，主打**超长文档/多页文档一键解析**能力。

---

## 技术架构

### 基础模型

基于 DeepSeek-OCR 衍生的 **3B MoE VLM**（视觉语言模型），采用端到端架构。

### 两种推理模式

| 模式 | base_size | image_size | crop_mode | 适用场景 |
|------|-----------|------------|-----------|---------|
| **gundam** | 1024 | 640 | True | 单图文档 |
| **base** | 1024 | 1024 | False | 多页/PDF |

### 关键参数

- `max_length`: **32768 tokens**（支持超长上下文）
- `no_repeat_ngram_size`: 35 — 防止重复输出
- `ngram_window`: 128（gundam）/ 1024（base）
- PDF 转图片：DPI 300

---

## 推理方式

### 1. Transformers（直接调用）

```python
from transformers import AutoModel, AutoTokenizer

model = AutoModel.from_pretrained(
    'baidu/Unlimited-OCR',
    trust_remote_code=True,
    torch_dtype=torch.bfloat16,
)
model = model.eval().cuda()

# 单图
model.infer(tokenizer, prompt='<image>document parsing.',
    image_file='your_image.jpg', output_path='./output',
    base_size=1024, image_size=640, crop_mode=True)

# 多页/PDF
model.infer_multi(tokenizer, prompt='<image>Multi page parsing.',
    image_files=['page1.png', 'page2.png'], output_path='./output')
```

### 2. SGLang（API Server）

```bash
python -m sglang.launch_server \
    --model baidu/Unlimited-OCR \
    --context-length 32768 \
    --port 10000
```

支持 OpenAI-compatible API：`/v1/chat/completions`

### 3. HuggingFace Spaces

AK 制作的在线 Demo：https://huggingface.co/spaces/baidu/Unlimited-OCR

---

## 支持格式

- ✅ 单图解析（jpg/png）
- ✅ 多图解析
- ✅ PDF（全页转图后解析）

---

## 依赖环境（Transformers）

```
torch==2.10.0
transformers==4.57.1
einops==0.8.2
addict==2.4.0
easydict==1.13
pymupdf==1.27.2.2
Pillow==12.1.1
matplotlib==3.10.8
psutil==7.2.2
```

---

## 本地部署硬件要求

| | **Unlimited-OCR** | **PaddleOCR** |
|---|---|---|
| **最低配置** | 16GB VRAM | CPU 可跑（~2GB RAM） |
| **推荐配置** | 24GB+ VRAM | 4-6GB VRAM |
| **模型规模** | 3B MoE VLM | PP-OCRv6: 1.5M~34.5M |
| **GPU 依赖** | **必须 GPU** | 可选（CPU 也能跑） |
| **推理速度（A100）** | ~56 tok/s | 0.13秒/页 |

### PaddleOCR 三档配置

| 档次 | 参数量 | 适用场景 |
|------|--------|---------|
| tiny | 1.5M | 边缘/移动端 |
| small | 7.7M | 端侧部署 |
| medium | 34.5M | 服务器主流 |

> Unlimited-OCR 基于 3B VLM，必须 GPU 才能运行；PaddleOCR 传统架构极小，CPU 即可。

---

## 与 PaddleOCR 区别

| | **PaddleOCR** | **Unlimited-OCR** |
|---|---|---|
| **架构** | 传统 OCR pipeline（检测+识别多阶段） | VLM 端到端 |
| **模型** | PP-OCRv4/v6 + PP-OCR-VL 多模型组合 | 3B MoE VLM |
| **单图处理** | 分段检测+识别 | 一次端到端理解 |
| **多页/长文档** | 每页分别处理再拼接 | **One-shot 整本解析** |
| **擅长** | 场景TextDetection、卡证识别 | 论文、报告、书籍整体解析 |

**类比**：
- PaddleOCR = 精密手术刀，精准定位每个文字区域
- Unlimited-OCR = 阅读理解高手，看整页/整本直接输出结构化内容

---

## 生态扩展（社区 Fork）

- `say4n/unlimited-ocr-container` — Docker 一键运行
- `PRITHIVSAKTHIUR/DeepSeek-OCR-2-Unlimited-OCR` — DeepSeek-OCR-2 融合版
- TongFlow 插件（Modal GPU）

---

## 论文

arXiv: https://arxiv.org/abs/2606.23050

---

## 总结

Unlimited-OCR 是近期 OCR 领域重磅开源，核心优势：
- **超长上下文**（32k tokens）一次性处理超长文档
- **多页/PDF 整本解析**无需切片
- **MIT 许可证**可商用

适用场景：批量解析 PDF/扫描件、提取文档整体内容、长篇论文理解。

如需精确文字坐标、卡证票证识别，仍推荐 PaddleOCR。

---

*Tags: #OCR #百度 #VLM #DeepSeek-OCR #文档理解 #开源*
