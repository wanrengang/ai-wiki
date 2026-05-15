# 邮箱配置 - Himalaya + QQ 邮箱

> 日期：2026-05-15
> 状态：✅ 完成

## 背景

需要为 OpenClaw 配置邮件管理能力，使用 Himalaya CLI 工具通过 IMAP/SMTP 管理邮箱。

## 过程

### 1. 尝试 163 邮箱（失败）

- 邮箱：`wanrengang1688@163.com`
- IMAP：`imap.163.com:993`
- SMTP：`smtp.163.com:465`
- 授权码：`FMumEwMN3iS26Zb6` → `ELuxynD35A7Sm2Cm`
- **问题**：登录成功但 SELECT INBOX 时持续返回 `Unsafe Login` 错误，163 安全系统对服务器 IP (`103.129.180.55`) 进行了限制
- **结论**：163 邮箱不支持从该服务器 IP 访问

### 2. 切换到 QQ 邮箱（成功）

- 邮箱：`365700955@qq.com`
- IMAP：`imap.qq.com:993`
- SMTP：`smtp.qq.com:465`
- 授权码：`jfdefensfhmubjja`
- **结果**：IMAP 读取和 SMTP 发送均正常

### 3. Himalaya 安装

- 从 GitHub Releases 下载 v1.2.0 二进制
- 路径：`~/.local/bin/himalaya`
- 配置文件：`~/.config/himalaya/config.toml`

### 4. 配置文件内容

```toml
[accounts.qq]
email = "365700955@qq.com"
display-name = "万仁刚"
default = true

backend.type = "imap"
backend.host = "imap.qq.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "365700955@qq.com"
backend.auth.type = "password"
backend.auth.raw = "jfdefensfhmubjja"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.qq.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "365700955@qq.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.raw = "jfdefensfhmubjja"

[accounts.qq.folder.alias]
inbox = "INBOX"
sent = "Sent Messages"
drafts = "Drafts"
trash = "Deleted Messages"
junk = "Junk"
```

### 5. 测试结果

| 功能 | 状态 |
|------|------|
| IMAP 读取邮件 | ✅ 正常 |
| 列出未读邮件 | ✅ 正常（89封未读） |
| SMTP 发送邮件 | ✅ 正常 |
| 发送后保存到已发送 | ⚠️ UID 错误（邮件实际发送成功） |

## 发现的问题

- **163 邮箱**：服务器 IP 被安全策略限制，IMAP 不可用
- **QQ 邮箱发送后保存**：QQ 服务器 APPEND 返回 UID 格式与 Himalaya 预期不符，但邮件实际已发出
- **Himalaya 需要 EDITOR 环境变量**：非交互模式下需设置 `EDITOR=cat`

## 后续

- 代理设置：`http://127.0.0.1:7890`，已确认代理不是 163 邮箱失败的原因
- 计划后续配置 Gmail 作为备用邮箱
