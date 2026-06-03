# 微软 Build 2026：Windows 内置 OpenClaw 深度研究

> **研究日期：** 2026年06月03日  
> **来源：** Microsoft Build 2026 / Windows Developer Blog / GitHub

---

## 核心事件

**Microsoft Build 2026**（6月2-3日，旧金山）正式宣布：

> OpenClaw 将原生内置于 Windows，成为微软 AI Agent 生态的核心组成部分。

OpenClaw 创始人 Peter Steinberger 亲自登台，与微软 CEO 纳德拉同台展示。

---

## 技术架构：MXC + OpenClaw

### Microsoft Execution Containers（MXC）

**MXC** 是微软推出的操作系统级 AI Agent 沙箱技术，是本次合作的技术核心：

- **定位**：跨平台（Windows + WSL）的策略驱动执行层
- **原理**：开发者声明 Agent 可访问的权限（文件、网络等），MXC 在运行时强制执行边界
- **隔离方式**：通过进程隔离、会话隔离等 OS 原生技术实现
- **企业特性**：与 Intune/Entra/Defender/Purview 集成，IT 管理员可统一管控

### OpenClaw × MXC

OpenClaw 通过 MXC 在 Windows 上实现**原生、安全的本地运行**：

- OpenClaw Node + Gateway 运行在 MXC 容器内，与宿主机隔离
- 系统安全得到保障，Agent 无法随意访问主机资源
- 企业可统一管控：定义一次策略，Windows 处处执行

---

## 两种部署路径

| 路径 | 说明 | 适用场景 |
|------|------|---------|
| **MXC（本地）** | OpenClaw 通过 MXC 原生运行在 Windows 上 | 想在本地 Windows 机器上运行 Agent |
| **openclaw-dev（云端）** | 微软官方云托管版本，部署到 Azure ephemeral sandbox | 需要多设备访问、始终在线、免维护 |

### openclaw-dev 详情

- 基于 Azure Container Apps + Foundry Models（默认 gpt-5-mini）
- 微软账号登录，无需 API Key
- 支持 Microsoft Teams 接入（可选）
- 6 分钟完成首次部署
- 仓库：github.com/microsoft/openclaw-dev

---

## 合作伙伴生态

Build 2026 上，NVIDIA 也宣布基于 MXC 构建 **OpenShell**——面向开发者的易部署 Agent 运行时包。

其他已接入 MXC 的合作伙伴：OpenAI、Manus、Nous Research。

---

## 对比：Windows 原生安装 vs MXC

| 维度 | 原生 Windows 安装 | MXC 集成（Build 2026 新方案） |
|------|-----------------|------------------------------|
| 隔离性 | 无 OS 级隔离 | OS 内核级强制隔离 |
| 企业管控 | 手动配置 | 与 Entra/Defender/Intune/Purview 深度集成 |
| 适用人群 | 开发者个人 | 企业 IT + 开发者 |
| 安装方式 | 手动安装 CLI | MXC SDK + companion app |

---

## Microsoft Scout：OpenClaw 的杀手级应用

Build 2026 宣布了 **Microsoft Scout**——基于 OpenClaw + WorkIQ 构建的个人 AI Agent：

- 理解用户的工作方式
- 主动处理会议准备、日程冲突、常规任务
- 无需询问即可完成
- 目前面向 Frontier 客户，未来扩大范围

---

## 研究结论

1. **微软战略**：将 OpenClaw 定位为 Windows AI Agent 战略的核心组件，而非简单集成
2. **核心价值**：MXC 解决了在 Windows 上安全运行 Agent 的企业级挑战
3. **生态影响**：OpenClaw 从开源项目升级为微软官方认证的 Agent 平台
4. **对企业意义**：未来 IT 部门可统一管控员工 Windows 设备上的 Agent 行为

---

## 参考链接

- Windows Developer Blog: https://blogs.windows.com/windowsdeveloper/2026/06/02/build-2026-furthering-windows-as-the-trusted-platform-for-development/
- Microsoft Official Blog: https://blogs.microsoft.com/blog/2026/06/02/microsoft-build-2026-be-yourself-at-work/
- GitHub (Cloud): https://github.com/microsoft/openclaw-dev
- GitHub (Windows Node): https://github.com/openclaw/openclaw-windows-node
- MXC SDK: https://github.com/microsoft/mxc

---

*标签：#OpenClaw #Windows #MXC #微软 #Build2026 #AI-Agent #企业安全*
