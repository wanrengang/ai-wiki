# 用 React 写视频，而不是用剪映：Remotion 让我重新理解了「视频制作」这件事

> 来源：AI剧-阿丙（微信公众号）
> 日期：2026年6月5日

## 核心观点

GitHub 上有一个 48.5k star 的项目，叫 **Remotion**。slogan 是：Make videos programmatically。用 React 写视频。

**这个工具可能是目前最接近「用程序员的思维做视频」的实践。**

## 01 它解决什么问题

传统的视频制作软件——Premiere、Final Cut、达芬奇——它们的操作逻辑是 **时间线 + 轨道**。

Remotion 完全不同。它把你的视频看成一个 React 组件——你有组件树、有 props、有 useEffect、有状态管理、有 CSS 动画、有 SVG 绘制、有 Canvas 渲染。

**你的视频是一行行代码，不是一个个时间轴上的轨道。**

这意味着你可以：
- 用**变量**控制视频里的数字——比如 GDP 数据，每次改一个数字，视频里所有相关的地方同步更新
- 用**函数**封装复用组件——比如标题卡片，做一次，后面每个视频都能调用
- 用**算法**生成视觉效果——比如数据可视化、动态图表、程序化生成的动画
- 用**API** 接入数据源——比如接一个天气 API，视频自动生成今日天气播报

## 02 为什么火？GitHub 48k star 的背后

Remotion 不是一个新项目，33,335 次 commits，v4 版本刚发。真正让它出圈的是几个重量级案例：

- **GitHub Unwrapped**：GitHub 年终回顾视频，每个开发者打开自己的 GitHub 主页，看到的是专属于自己的年度贡献视频
- **Fireship**：著名的「This video was made with code」，源代码也是 Remotion
- **Banger.Show**：3D 音乐可视化工具
- **大量公司内部**：数据报表视频化、产品演示自动化、批量生成营销素材

**需要大量个性化、自动化、数据驱动的视频。传统视频工具无法规模化做到的事情，代码可以。**

## 03 对我冲击最大的一个场景

作者在做 AI 短剧时，最头疼的是**分镜脚本定稿之后，如何高效地把静态的分镜图变成有节奏感的视频**。

Remotion 的启发在于「用**参数驱动视觉生成**」这个思路：
- 片头的数据动画、战损统计、战役时间线这些元素，用 Remotion 写一个组件，改参数就能自动生成

**一旦这套组件库搭好，你改一个数字，整段视频自动更新。这才是真正的「参数化视觉」。**

## 04 它的生态比你想象的完整

- **Remotion Player**：把视频嵌入到 Web 应用里
- **Remotion Lambda**：在云端渲染视频，不用本地机器
- **Remotion Editor Starter**：完整视频编辑器模板
- **Remotion Recorder**：录屏转视频
- **Remotion Timeline**：时间轴编辑器
- **800+ 模板和示例**

## 05 诚实的缺点

- **学习曲线不低**：你得懂 React、懂 CSS 动画、懂 TypeScript
- **渲染耗时**：本地渲染视频需要机器性能，云端渲染（Lambda）需要费用
- **复杂特效有局限**：对于复杂的 3D 场景和实拍视频的处理不是最好选择
- **许可证需关注**：个人和 3 人以下团队免费商用，4 人以上需要购买商业许可证
- **不适合实时交互的视频**：它是「渲染」逻辑，不是「实时播放」逻辑

## 06 Remotion 和 HyperFrames 的关系

**它们不是竞争关系，是互补关系。**

| | Remotion | HyperFrames |
|---|---|---|
| 定位 | **内容生产层** | **合成剪辑层** |
| 核心 | 程序化、参数化、批量生成 | 实时预览、交互动画、浏览器直出 |
| 渲染逻辑 | 确定性渲染，同一组参数永远输出同一个视频 | 合成渲染，改 HTML 立刻看到效果 |
| 强项 | 数据驱动、程序化生成、批量生产 | 实时合成、交互动画、快速迭代 |

## 07 怎么开始

```bash
npx create-video@latest
```

- GitHub：https://github.com/remotion-dev/remotion（48.5k star，MIT 协议）
- 官网：https://remotion.dev（文档非常完整，800+ 页）

## 08 写在最后

**视频不一定是「拍」出来的，也可以是「算」出来的。**

当视频的内容是数据驱动的、是程序化生成的、是高度个性化的，「拍」反而成了最笨的方法。你需要一个渲染引擎，而不是一台摄像机。

---

**相关项目**：
- [remotion-dev/github-unwrapped](https://github.com/remotion-dev/github-unwrapped) - GitHub Unwrapped 项目
- [VoxCPM/HyperFrames](/concepts/VoxCPM-HyperFrames视频自动化方案.md) - 视频自动化合成工具
