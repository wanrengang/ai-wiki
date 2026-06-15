# Scrapling — 自适应网页爬虫框架

- **GitHub:** https://github.com/D4Vinci/Scrapling
- **文档:** https://scrapling.readthedocs.io
- **Star:** 59,000+
- **作者:** D4Vinci
- **许可证:** MIT
- **Python 版本:** 3.10+
- **标签:** #网页爬虫 #Python #AI工具 #MCP

---

## 核心定位

> An adaptive Web Scraping framework that handles everything from a single request to a full-scale crawl.

Scrapling 是一个**自适应网页爬虫框架**，解决网页爬虫领域二十年的痛点：**网站改版 → 脚本失效**。

它的解析器能"学习"网站结构变化，当页面更新后自动重新定位目标元素，而无需手动修改选择器。

---

## 核心能力

### 1. 自适应解析（Adaptive Parsing）
- `auto_save=True`：首次抓取时保存元素特征
- `adaptive=True`：当网站结构变化时，自动用相似度算法重新定位元素
- 网站改版后解析器能自动适应，大幅降低维护成本

### 2. 多种 Fetcher 类型

| Fetcher | 用途 |
|---------|------|
| `Fetcher` | 快速 HTTP 请求，可伪装 TLS 指纹、模拟浏览器请求头 |
| `StealthyFetcher` | 高级隐身模式，可绕过 Cloudflare Turnstile 等反爬机制 |
| `DynamicFetcher` | 完整浏览器自动化（基于 Playwright Chromium） |

### 3. Spider 爬虫框架
- Scrapy 风格的 Spider API（`start_urls`、`parse` 回调、`Request/Response` 对象）
- 异步并发爬取，可配置并发数和域名限速
- 多 Session 支持：同一蜘蛛内混合使用 HTTP 请求和浏览器 Session
- **暂停/恢复**：基于检查点的持久化，Ctrl+C 优雅关闭，重启后从断点继续
- **流式模式**：`async for item in spider.stream()` 实时获取数据

### 4. MCP Server（AI 集成）
- 内置 MCP Server，可与 AI（Claude、Cursor 等）配合使用
- AI 爬取目标内容后再传给 AI 处理，减少 token 消耗
- 视频演示：https://www.youtube.com/watch?v=qyFk3ZNwOxE

### 5. 反爬与代理
- 内置 Cloudflare Turnstile 绕过
- 代理轮换：`ProxyRotator` 支持循环/自定义策略
- Robots.txt 遵守：`robots_txt_obey` 参数
- DNS 泄漏防护：DNS-over-HTTPS（经由 Cloudflare DoH）

---

## 性能对比

| 库 | 解析耗时 (ms) | 相对速度 |
|---|--|--|
| **Scrapling** | 2.02 | 1x（基准）|
| Parsel/Scrapy | 2.04 | 1.01x |
| Raw Lxml | 2.54 | 1.26x |
| PyQuery | 24.17 | ~12x |
| BeautifulSoup + Lxml | 1584 | **~784x** |
| BS4 + html5lib | 3392 | ~1679x |

Scrapling 比 BeautifulSoup 快 **784 倍**，与 Scrapy/Parsel 相当。

---

## 安装

```bash
# 基础版（仅解析器）
pip install scrapling

# 完整版（含浏览器自动化）
pip install "scrapling[all]"
scrapling install  # 自动下载 Chromium + 所有依赖
```

---

## 快速上手

### CLI（零代码）
```bash
scrapling extract get 'https://example.com' output.md
```

### 自适应解析
```python
from scrapling.fetchers import StealthyFetcher

StealthyFetcher.adaptive = True
page = StealthyFetcher.fetch('https://example.com', headless=True, network_idle=True)
products = page.css('.product', auto_save=True)  # 保存元素特征
products = page.css('.product', adaptive=True)  # 网站改版后自动重定位
```

### 构建爬虫
```python
from scrapling.spiders import Spider, Response

class MySpider(Spider):
    name = "demo"
    start_urls = ["https://example.com/"]

    async def parse(self, response: Response):
        for item in response.css('.product'):
            yield {"title": item.css('h2::text').get()}

result = MySpider().start()
result.items.to_json("output.json")
```

---

## 适用场景

✅ **适合使用 Scrapling 的场景：**
- 想让 AI Agent 自动获取网页信息
- 做公开数据研究、学术调研需要批量采集
- 想从 Scrapy 迁移到更现代的框架
- 偶尔提取公开网页但不会编程（CLI 模式）

❌ **不适合的场景：**
- 对性能和内存极度敏感（浏览器模式较重）
- 已经深度绑定 Scrapy 中间件生态
- 不了解 robots.txt 等基本规则

---

## 与 Playwright 的关系

> Scrapling 不是要取代 Playwright——它站在 Playwright 的肩膀上。

- **Playwright** 负责浏览器操作
- **Scrapling** 负责解析逻辑、自适应选择器、CLI、Spider 框架、MCP Server

---

## 技术亮点

1. **自适应元素追踪**：智能相似度算法，网站改版后自动重定位
2. **隐身指纹伪造**：绕过 Cloudflare Turnstile 等反爬机制
3. **异步流式爬取**：实时返回数据，支持 UI 展示和长时爬取
4. **开发缓存模式**：`crawldir` 参数缓存响应，调 `parse()` 逻辑时不反复请求
5. **完整类型提示**：92% 测试覆盖率，PyRight + MyPy 全面扫描

---

## 参考资料

- GitHub: https://github.com/D4Vinci/Scrapling
- 文档: https://scrapling.readthedocs.io
- MCP Server: https://scrapling.readthedocs.io/en/latest/ai/mcp-server.html
- Claude 集成演示: https://www.youtube.com/watch?v=qyFk3ZNwOxE
