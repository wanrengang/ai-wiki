# 微软正在协助将OpenClaw打造为适合企业使用的解决方案！

> 来源：微信公众号 - One的AI工具箱  
> 链接：https://mp.weixin.qq.com/s/LFgSqDoW06IiL76Ox8pJCA  
> 作者：One 掌柜  
> 日期：2026年5月13日

## 核心要点

- **PR #78678**：OpenClaw 新增统一的 `oc://` 寻址方案和 `openclaw path` 命令
- **规模**：86 个文件、39 条评论、22 个 commits、一万四千多行改动
- **意义**：这不是小功能，而是补足了 Agent 系统中最脏最难的底层——工作区结构化文件的统一寻址和编辑能力
- **方向**：微软 Project Lobster 团队在帮助 OpenClaw 往企业化方向推进

## 文章总结

### 这次加了什么

1. **统一路径规则**：`oc://gateway.jsonc/version` 可以精确定位文件中的具体节点，而非"打开文件看看"
2. **CLI 子命令**：
   - `openclaw path resolve`
   - `openclaw path find`
   - `openclaw path set`
   - `openclaw path validate`
   - `openclaw path emit`
3. **支持的格式**：Markdown、JSONC、JSONL、YAML 等结构化文件格式统一收敛到同一套路径规则

### 为什么这层很关键

Agent 进入真实工作流后最大的痛苦不是"不会生成"，而是"老是乱动"：
- 改一个配置，顺手把文件格式重排一遍
- 补一个字段，把旁边注释搞没了
- 不同插件各有一套自己的定位方式

统一寻址底座解决的是：结构化文件能不能被**稳定、统一、可编程地操作**。

### 对普通用户的意义

- 只改一个配置值，不想动别的
- 只找某个节点，不想全文件自己 parse
- 让插件和 Agent 共用一套定位规则
- 从终端直接验证工作区某个结构是否正确
- 写自动化时不用每次都自己造文件操作轮子

### 局限性

当前 JSONC 写入可能丢 comments 和部分原始格式（重新 emit 而非字节级保留式编辑），但底座已搭好，后续会继续补全。

### 文章观点

> OpenClaw 不是在做一个"演示时很厉害"的 Agent，而是在往一个更难的方向走：做一个能长期接活、能接复杂环境、能接企业流程的系统。

---

标签：#OpenClaw #企业级AI #Agent系统 #微软 #开源
