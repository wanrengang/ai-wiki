# 浏览器自动化：从GUI到OpenCLI

> 来源：微信公众号「大淘宝技术」
> 作者：明径
> 日期：2026-05-22

## 核心问题

大量业务系统跑在浏览器里——运营配置后台、工单处理系统、发布运维平台。Agent 想操控浏览器，路并不好走：前端 UI 自动化不稳定、效率低。

## OpenCLI 思路

**不跟网页界面较劲，直接抓它背后的 API。**

浏览器里看到的数据，本质上都是前端从某个接口拿回来的。把这个接口找出来、把请求复现出来，比点按钮靠谱得多。

## 快速上手

```bash
npm install -g @jackwener/opencli
```

常用命令：
```bash
opencli list                    # 查看所有命令
opencli list -f yaml           # 以 YAML 列出所有命令
opencli hackernews top --limit 5  # 公共 API，无需浏览器
opencli bilibili hot --limit 5    # 浏览器命令
opencli zhihu hot -f json        # JSON 输出
```

## AI Agent 探索工作流（六步）

| 步骤 | 工具 | 做什么 |
|------|------|--------|
| 0 | browser_navigate | 导航到目标页面 |
| 1 | browser_snapshot | 观察可交互元素 |
| 2 | browser_network_requests | 首次抓包，筛选 JSON API 端点 |
| 3 | browser_click + browser_wait_for | 模拟交互（点字幕/评论/关注等按钮）|
| 4 | browser_network_requests | 二次抓包，对比找新触发的 API |
| 5 | browser_evaluate | fetch(url, {credentials:'include'}) 验证返回结构 |
| 6 | — | 基于确认的 API 写适配器 |

## 懒加载机制

> **你（AI Agent）必须通过浏览器打开目标网站去探索！**

很多 API 是懒加载的——用户必须点击某个按钮/标签才会触发网络请求。字幕、评论、关注列表等深层数据不会在页面首次加载时出现在 Network 面板中。

## 五级认证策略

用 `cascade` 命令自动探测：

```bash
opencli cascade https://api.example.com/hot
```

策略决策树：

1. **Tier 1: public** — 直接 fetch(url) 能拿到数据（公开 API）
2. **Tier 2: cookie** — fetch(url, {credentials:'include'}) 带 Cookie 能拿到（最常见）
3. **Tier 3: header** — 加上 Bearer / CSRF header 后能拿到（如 Twitter ct0 + Bearer）
4. **Tier 4: intercept** — 网站有 Pinia/Vuex Store（Store Action + XHR 拦截）
5. **Tier 5: ui** — UI 自动化（最后手段）

## 适配器

两种格式：
- **YAML** (`.yaml`)：简单场景（Cookie/Public auth）
- **TypeScript** (`.ts`)：复杂场景（Intercept capture、Header auth）

路径：`~/.opencli/clis/{site}/{command}.yaml|.ts`

## 自动生成CLI流程

AI 原生生成 CLI 流程：

1. **探索与分析**：`explore` 深度抓取页面、自动滚动、拦截网络请求、识别框架与状态管理
2. **策略选择**：根据鉴权头/签名等特征自动选择策略
3. **适配器合成**：`synthesize` 基于探索产物生成候选 YAML
4. **测试与验证**：`generate` 串联探索→合成→注册→验证

## Record 操作录制

`opencli record` 采用"浏览器录制 - 智能回放"模式：捕获用户在目标 URL 上的交互行为及产生的网络请求，自动生成可复用的 CLI 命令。

**当前局限性**：仅捕获请求元数据（url, method, body: responseBody），未能完整提取 POST/PUT 等写操作的 Request Body，导致写操作类接口的自动化闭环中断。

## 核心观点

> **过去的软件竞争界面，未来的软件竞争可调用性。**

- 以前评价 SaaS 看界面顺不顺；Agent 不欣赏按钮，只在乎能不能稳定调用
- GUI 是给人用的，API 是能力底座，Agent 最喜欢更清晰的执行面：命令、参数、返回值、失败原因
- 未来软件的新竞争维度：谁更容易被 Agent 理解、调用、验证，接进工作流

## 相关文章

- 3DXR技术、终端技术、音视频技术、服务端技术、技术质量、数据算法（公众号拓展阅读）
