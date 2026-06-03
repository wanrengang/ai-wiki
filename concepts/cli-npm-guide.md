# Node.js CLI 与 NPM 实战指南

> 本文整理自 2026-05-17 与老万的对话

---

## 一、CLI 开发实战

### 1.1 技术选型

| 语言 | 典型 CLI | 特点 |
|------|---------|------|
| Python | `gh`, `aws`, `docker` | 上手快、库丰富 |
| Node.js | `npm`, `vercel`, `wrangler` | JSON 处理顺、npm 生态成熟 |
| Go | `kubectl`, `terraform` | 编译单二进制、部署简单 |
| Rust | `bat`, `ripgrep` | 性能高、内存安全 |

**结论**：内部平台封装用 **Node.js** 最合适，和 OpenClaw 同语言，集成方便。

### 1.2 核心依赖

- **commander** — 命令行参数解析（最流行）
- **fetch / node-fetch** — HTTP 请求

### 1.3 项目结构

```
my-cli/
├── bin/
│   └── run.js        # 入口文件（加 shebang）
├── src/
│   ├── index.js      # 主程序
│   ├── commands/
│   │   ├── platform.js
│   │   └── data.js
│   └── api/
│       └── client.js
└── package.json
```

### 1.4 入口文件 `bin/run.js`

```javascript
#!/usr/bin/env node

const { Command } = require('commander');
const platform = require('../src/commands/platform');
const data = require('../src/commands/data');

const program = new Command();

program
  .name('my-cli')
  .description('内部平台 CLI')
  .version('1.0.0');

program.addCommand(platform);
program.addCommand(data);

program.parse();
```

### 1.5 子命令示例 `src/commands/platform.js`

```javascript
const { Command } = require('commander');
const { apiGet, apiPost } = require('../api/client');

const platform = new Command('platform');
platform.description('平台相关操作');

// 查询
platform
  .command('get <id>')
  .description('根据 ID 获取资源')
  .action(async (id) => {
    try {
      const result = await apiGet(`/platform/resource/${id}`);
      console.log(JSON.stringify(result, null, 2));
    } catch (err) {
      console.error('❌ 错误:', err.message);
      process.exit(1);
    }
  });

// 创建
platform
  .command('create')
  .description('创建新资源')
  .requiredOption('-n, --name <name>', '资源名称')
  .requiredOption('-t, --type <type>', '资源类型')
  .action(async (options) => {
    try {
      const result = await apiPost('/platform/resource', {
        name: options.name,
        type: options.type
      });
      console.log('✅ 创建成功:', JSON.stringify(result, null, 2));
    } catch (err) {
      console.error('❌ 错误:', err.message);
      process.exit(1);
    }
  });

module.exports = platform;
```

### 1.6 API 客户端 `src/api/client.js`

```javascript
const API_BASE = process.env.MY_CLI_API_URL || 'http://internal-platform/api';
const API_TOKEN = process.env.MY_CLI_TOKEN;

async function request(method, path, body = null) {
  const opts = {
    method,
    headers: {
      'Authorization': `Bearer ${API_TOKEN}`,
      'Content-Type': 'application/json',
    },
  };

  if (body) opts.body = JSON.stringify(body);

  const res = await fetch(`${API_BASE}${path}`, opts);

  if (!res.ok) {
    const err = await res.text();
    throw new Error(`API 错误 ${res.status}: ${err}`);
  }

  return res.json();
}

const apiGet  = (path) => request('GET', path);
const apiPost = (path, body) => request('POST', path, body);

module.exports = { apiGet, apiPost };
```

### 1.7 `package.json` 关键配置

```json
{
  "name": "@your-org/my-cli",
  "version": "1.0.0",
  "description": "内部平台 CLI",
  "main": "bin/run.js",
  "bin": {
    "my-cli": "./bin/run.js"
  },
  "keywords": ["cli", "internal"],
  "license": "MIT",
  "dependencies": {
    "commander": "^11.0.0"
  }
}
```

### 1.8 本地测试

```bash
npm install
npm install -g .   # 本地安装全局测试
my-cli --help      # 验证命令
```

---

## 二、NPM 发布与共享

### 2.1 NPM 账号注册流程

1. 访问 [npmjs.com](https://www.npmjs.com) 注册账号
2. 完成邮箱验证
3. 本地登录：`npm login`

### 2.2 发布到公共 NPM

```bash
# 打 Git Tag
git tag v1.0.0
git push origin v1.0.0

# 发布
npm publish --access public
```

### 2.3 版本号规则（SemVer）

```
v1.0.0
 │  │  │
 │  │  └── patch：修复 bug
 │  └───── minor：新功能（兼容旧版）
 └──────── major：破坏性变更
```

### 2.4 常用 NPM CLI 命令

```bash
npm login              # 登录
npm logout             # 登出
npm whoami              # 查看当前账号
npm publish            # 发布
npm install -g .      # 本地安装测试
npm version patch      # 自动 increment 版本号
npm unpublish          # 下架（谨慎用）
```

---

## 三、内部共享方案

### 方案一：Git URL 安装（最推荐）

```bash
# 默认安装 master 分支
npm install -g git+https://gitlab.your-company.com/ai-team/my-cli.git

# 指定版本（Tag）
npm install -g git+https://gitlab.your-company.com/ai-team/my-cli.git#v1.0.0

# 指定分支
npm install -g git+https://gitlab.your-company.com/ai-team/my-cli.git#develop

# SSH 地址（需配置 SSH Key）
npm install -g git+ssh://git@gitlab.your-company.com:ai-team/my-cli.git
```

**前提**：项目的 `package.json` 必须有 `bin` 字段

### 方案二：Verdaccio 私有 NPM 仓库

```bash
# Docker 启动
docker run -d \
  --name verdaccio \
  -p 4873:4873 \
  -v /data/verdaccio:/var/lib/verdaccio \
  verdaccio/verdaccio

# 团队成员配置 registry
npm config set registry http://your-server:4873

# 发布到私有仓库
npm publish
```

### 方案三：直接共享目录

```bash
# 开发者在本地执行
cd my-cli && npm link

# 其他成员 cp 整个目录后执行
cd my-cli && npm link
```

---

## 四、内部共享方案对比

| 方案 | 适合规模 | 优点 | 缺点 |
|------|---------|------|------|
| Git URL 安装 | 小团队 (<10人) | 零基础设施，简单 | 每次拉整个仓库 |
| Verdaccio 私有仓 | 中大型团队 | 接近公共 NPM 体验 | 需维护服务 |
| 直接共享目录 | 临时使用 | 最简单 | 更新麻烦 |

---

## 五、设计要点（Agent 友好）

1. **输出 JSON**：方便 Agent 解析结果
   ```javascript
   console.log(JSON.stringify({ status: 'ok', data: result }));
   ```

2. **环境变量传 Token**：避免明文
   ```javascript
   const API_TOKEN = process.env.MY_CLI_TOKEN;
   ```

3. **清晰错误信息**：Agent 看得懂
   ```javascript
   console.error(JSON.stringify({ status: 'error', message: err.message }));
   process.exit(1);
   ```

4. **统一命令风格**：`<resource> <action> [options]`
   ```
   my-cli platform get 12345
   my-cli platform create --name "测试" --type "server"
   ```

---

## 六、飞书 CLI 参考

`lark-cli` 是 Node.js 写的，安装路径：
```
/home/wrg/.hermes/node/lib/node_modules/@larksuite/cli/scripts/run.js
```

可作为内部 CLI 项目架构参考。
