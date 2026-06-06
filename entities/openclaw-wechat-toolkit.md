---
title: OpenClaw 微信工具包
created: 2026-03-08
updated: 2026-03-08
type: entity
tags: [openclaw, wechat, toolkit, content, automation]
sources: [raw/articles/openclaw-skill-wechat-toolkit深度研究报告.md]
confidence: high
---

# OpenClaw 微信工具包

> 微信公众号一站式工具包，集成搜索→下载→洗稿→发布四大功能

**项目仓库：** https://github.com/Mr-Q526/openclaw-skill-wechat-toolkit  
**项目状态：** 新项目（2026-03-04 创建）  
**许可证：** MIT License

---

## 项目概览

| 属性 | 值 |
|------|-----|
| 项目名称 | openclaw-skill-wechat-toolkit |
| 所有者 | Mr-Q526 |
| 语言 | JavaScript (Node.js) |
| 创建时间 | 2026-03-04 |
| 最新更新 | 2026-03-05 |
| 开放 Issues | 0 个 |

---

## 核心功能

### 四大模块

| 模块 | 功能 | 说明 |
|------|------|------|
| 🔍 搜索 | 关键词搜索公众号文章 | 基于搜狗微信搜索，支持抓取正文 |
| 📰 下载 | 下载文章内容/配图/视频 | 输出 Markdown + HTML + 媒体文件 |
| ✍️ 洗稿 | AI 去痕迹 + 原创改写 | 结构重组、语言改写、SEO 优化 |
| 📱 发布 | 一键发布到公众号草稿箱 | 基于 wenyan-cli，支持主题和视频嵌入 |

---

## 技术架构

### 技术栈

```
openclaw-skill-wechat-toolkit
├─ Node.js ≥ 18 (运行环境)
├─ Cheerio (HTML 解析)
├─ Puppeteer (网页抓取)
├─ @wenyan-md/cli (公众号发布)
├─ OpenClaw (AI 助手平台)
└─ 微信公众号 API (发布接口)
```

### 项目结构

```
wechat-toolkit/
├── SKILL.md                          # OpenClaw Skill 定义
├── README.md                         # 项目说明
├── example.md                        # 示例文章
├── references/                       # 参考文档
│   ├── themes.md                     # 主题配置说明
│   └── troubleshooting.md            # 故障排查指南
└── scripts/                          # 脚本目录
    ├── search/                       # 搜索模块
    ├── downloader/                   # 下载模块
    └── publisher/                   # 发布模块
```

---

## 模块详解

### 搜索功能

**使用方法：**
```bash
node scripts/search/search_wechat.js "关键词"

# 高级搜索
node scripts/search/search_wechat.js "关键词" -n 5 -c
node scripts/search/search_wechat.js "关键词" -n 20 -o result.json
```

**参数说明：**

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `-n, --num` | 返回数量 | 10 (最大 50) |
| `-o, --output` | 输出 JSON 文件路径 | 无 |
| `-r, --resolve-url` | 解析微信文章真实链接 | 自动启用 |
| `-c, --fetch-content` | 抓取文章正文 | 自动启用 -r |

---

### 下载功能

**使用方法：**
```bash
# 首次设置
node scripts/downloader/download.js --set-output ~/Downloads/wechat-articles

# 完整下载（含图片、视频）
node scripts/downloader/download.js "https://mp.weixin.qq.com/s/xxx"

# 仅下载文本
node scripts/downloader/download.js "https://mp.weixin.qq.com/s/xxx" --no-image --no-video
```

**输出结构：**
```
<下载目录>/<文章标题>/
├── content/article.html          # 完整 HTML
├── metadata.json                  # 元数据
├── images/                        # 配图文件夹
└── videos/                        # 视频/音频文件夹
```

---

### 洗稿功能

**使用方式：** 作为 OpenClaw Skill 使用，通过自然语言指令触发 AI 改写

```
"帮我洗稿这篇文章"
"改写成原创"
"降低查重率"
"去掉 AI 味"
```

**改写策略：**
- 结构重组：调整段落顺序、重新组织逻辑
- 语言改写：同义词替换、句式变换
- 标题优化：生成吸引人的新标题
- 开头重写：重写导语，吸引读者
- SEO 优化：关键词优化、提升搜索排名

---

### 发布功能

**环境配置：**
```bash
export WECHAT_APP_ID=your_app_id
export WECHAT_APP_SECRET=your_app_secret
```

**发布文章：**
```bash
# 方式 1：Bash 脚本
bash scripts/publisher/publish.sh /path/to/article.md

# 方式 2：wenyan CLI
wenyan publish -f article.md -t lapis -h solarized-light

# 方式 3：Python 脚本（含视频）
python3 scripts/publisher/publish_with_video.py /path/to/article.md
```

---

## 完整工作流程

**步骤 1：搜索相关文章**
```bash
node scripts/search/search_wechat.js "AI Agent 架构" -n 10 -c -o results.json
```

**步骤 2：下载目标文章**
```bash
node scripts/downloader/download.js "https://mp.weixin.qq.com/s/xxx"
```

**步骤 3：使用 OpenClaw 洗稿**
```
在 OpenClaw 中：
"帮我把 ~/Downloads/wechat-articles/文章标题/content/article.html 改写成原创"
```

**步骤 4：发布到公众号**
```bash
bash scripts/publisher/publish.sh ~/Downloads/wechat-articles/改写后文章/article.md
```

---

## 安装配置

### 前置要求

- Node.js ≥ 18
- Google Chrome（下载模块需要）
- OpenClaw 运行环境
- 微信公众号开发者权限

### 安装步骤

1. 克隆到 OpenClaw skills 目录
```bash
git clone git@github.com:Mr-Q526/openclaw-skill-wechat-toolkit.git skills/wechat-toolkit
```

2. 安装依赖
```bash
npm install -g cheerio
npm install -g @wenyan-md/cli
```

3. 配置微信公众号（获取 AppID 和 AppSecret，设置 IP 白名单）

---

## 故障排查

| 问题 | 解决方法 |
|------|---------|
| IP 不在白名单 | `curl ifconfig.me` → 添加到公众号后台 |
| wenyan 未安装 | `npm install -g @wenyan-md/cli` |
| 环境变量未设置 | `export WECHAT_APP_ID=xxx` |
| 缺少 frontmatter | 添加 title + cover 字段 |
| 40001 token 失效 | 使用 `publish_with_video.py` |

---

## ⚠️ 法律与道德

- 仅供个人学习使用
- 请遵守相关版权法规
- 搜索功能内置防封禁机制（随机 UA、请求延迟）
- 请勿高频使用
- 尊重原作者版权


## 相关概念

- [[openclaw]] — 工具包的宿主平台
- [[openclaw-ecosystem]] — OpenClaw 生态整体概览
- [[ai-agent]] — AI Agent 通用概念
