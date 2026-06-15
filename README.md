# Agent Smith — 你的 AI 工具配置管家

> 一键切换 Claude Code 和 Hermes 的配置预设，实时追踪 Token 消耗，让多环境开发不再手忙脚乱。

---

## 它是谁

Agent Smith 是一款 macOS 桌面应用，专为同时使用多款 AI 编程工具（Claude Code、Hermes）的开发者设计。如果你经常在不同项目之间切换 API Key、模型版本或环境变量，Agent Smith 能让你告别手动改文件的烦恼。

---

## 核心功能

### 1. 配置预设管理 — "一键换环境"

工作中你可能同时维护多个项目：
- 公司项目用企业版 Claude API
- 个人项目用 OpenRouter 省钱
- 测试环境用本地 Ollama

Agent Smith 让你为每个工具创建**多套配置预设**，点一下就能切换生效，无需再手动编辑配置文件。

**支持的工具：**
- **Claude Code** — 管理 `settings.json` 中的环境变量（API Key、模型选择等）
- **Hermes** — 同时管理 `config.yaml` 的模型配置和 `~/.hermes/.env` 的环境变量

**预设能做什么：**
- 创建、编辑、复制、删除预设
- 点击"启用"立即写入真实配置文件
- 自动备份原始文件，改错了随时恢复
- 编辑时自动识别当前服务商，智能推荐对应的环境变量名

### 2. Token 用量分析 — "看清每一笔开销"

Agent Smith 会自动读取 Claude Code 和 Hermes 的本地日志，为你汇总 Token 消耗情况：

**总览面板：**
- 总处理 Token 数（带动态数字动画）
- 请求次数统计
- 缓存命中率（绿色进度条直观展示）

**四大维度拆解：**
| 维度 | 含义 |
|------|------|
| Fresh Input | 全新输入 Token |
| Output | AI 输出 Token |
| Creation | 缓存创建 Token |
| Hit | 缓存命中 Token |

**两种视图：**
- **按模型分组** — 看哪个模型最费 Token
- **用量日志** — 逐条查看每次请求的明细

**时间范围：**
今天 / 昨天 / 最近 7 天 / 最近 30 天 / 自定义日期区间

### 3. 智能自动刷新

打开 Token 分析页面时，Agent Smith 可以按设定间隔自动扫描新日志（默认 30 秒），数据实时更新。离开页面自动停止刷新，不浪费系统资源。

---

## 界面一览

```
┌─────────────────────────────────────────┐
│  Agent Smith                            │
├──────────┬──────────────────────────────┤
│          │                              │
│  Agents  │   Claude Code / Hermes       │
│  ──────  │   ┌────────────────────┐     │
│  🤖 CC   │   │ 预设列表            │     │
│  ⚡ Hermes│   │ ○ Default (Active) │     │
│          │   │ ○ Work             │     │
│  Stats   │   │ ○ Personal         │     │
│  ──────  │   └────────────────────┘     │
│  📊 Token│                              │
│          │   [New Profile]              │
│  ──────  │                              │
│  ⚙️ 设置 │                              │
│  ℹ️ 关于 │                              │
│          │                              │
└──────────┴──────────────────────────────┘
```

---

## 典型使用场景

**场景一：工作/个人项目切换**
> 早上到公司，打开 Agent Smith，点击"Work"预设启用公司 API Key；晚上回家切换"Personal"预设，自动切到个人 OpenRouter 账号。

**场景二：模型 A/B 测试**
> 为同一个项目创建 "Claude-4" 和 "Claude-3.7" 两个预设，快速切换对比效果。

**场景三：监控用量防超支**
> 打开 Token 页面，看今天已经用了多少 Token，缓存命中率如何，哪个模型最烧钱。

**场景四：新环境快速配置**
> 复制现有预设，改几个关键值，秒级生成新环境配置。

---

## 安全与隐私

- **纯本地运行** — 所有数据存储在你的 Mac 上，不会上传到任何服务器
- **自动备份** — 每次写入配置文件前自动备份原文件
- **敏感信息保护** — API Key 等敏感字段在界面中自动隐藏

---

## 系统要求

- macOS 13+（Apple Silicon）
- 已安装 Claude Code 或 Hermes（可选，未安装也能管理配置）

---

## 发版签名（维护者）

从 GitHub 下载的应用会被系统打上 **隔离（quarantine）** 标记。若未做 **Developer ID 签名 + Apple 公证（notarization）**，macOS 会提示「已损坏，无法打开」——这不是应用真的坏了，而是 Gatekeeper 拦截未公证软件。

`make release` 通过 **packages/deploy-cli**（Bun + chalk，彩虹步骤 UI）执行：**签名 → 打包 DMG/ZIP → 公证 → 上传**。

```bash
# 发版（每次 make 会先清空 node_modules 再 bun install，对齐干净环境）
SKIP_NOTARIZE=1 make release   # 未付费开发者计划
make release                   # 已配置 packages/deploy-cli/scripts/release.env
```

### 前提：Apple Developer Program（$99/年）

钥匙串里的 **Apple Development** 证书是 Xcode 免费账号自带的，**只能本机调试**，不能用于对外分发。

若打开 [Certificates](https://developer.apple.com/account/resources/certificates/list) 看到 *「Access Unavailable… only for developers enrolled in a developer program」*，说明当前 Apple ID **尚未加入付费开发者计划**。需要先：

1. 打开 [developer.apple.com/programs/enroll](https://developer.apple.com/programs/enroll) 注册（个人或公司，$99/年）
2. 审核通过后，再创建 **Developer ID Application** 证书并导入钥匙串
3. 配置公证凭据后发版

在此之前，**无法**让 GitHub 下载的包像商业软件一样免确认直接打开——这是 macOS 策略，不是打包脚本能绕过的。

### 未付费时的现实选择

| 方案 | 适合场景 |
|------|----------|
| **自己用** | `make build` 本机构建，或 `SKIP_NOTARIZE=1 make release` 后本机 `xattr -cr` |
| **发给熟人** | Release 附说明：安装后终端执行 `xattr -cr "/Applications/Agent Smith.app"`，或右键 → 打开（首次） |
| **开源让用户自编译** | `git clone` + `make build`，本机编译无 quarantine |
| **付费加入开发者计划** | 唯一正规、可大规模分发的方案 |

### 已加入开发者计划后的一次性配置

1. 创建 **Developer ID Application** 证书，导入钥匙串
2. 在 [appleid.apple.com](https://appleid.apple.com) 生成 **App 专用密码**
3. 复制配置模板并填写：

```bash
cp packages/deploy-cli/scripts/release.env.example packages/deploy-cli/scripts/release.env
# 编辑 NOTARY_APPLE_ID / NOTARY_TEAM_ID / NOTARY_PASSWORD
```

4. 在 `main` 分支执行 `make release`

### 临时本机测试（不公证）

```bash
SKIP_NOTARIZE=1 make release
```

未公证的包**只能你自己本机用**，发给别人仍会被拦截。

### 用户侧临时绕过（未付费发版时必需告知用户）

安装后执行：

```bash
xattr -cr "/Applications/Agent Smith.app"
```

或 Finder 中 **右键 → 打开**（仅首次）。完成付费签名+公证后，用户不再需要这些步骤。

---

## 快速开始

1. 从 [Releases](https://github.com/wxyy-org/agent-smith-release/releases) 下载 **Agent Smith.dmg**（推荐）或 **Agent Smith.zip**
   - **DMG**：打开后将左侧 **Agent Smith.app** 拖入右侧 **Applications（应用程序）**
   - **ZIP**：解压后将 **Agent Smith.app** 拖入「应用程序」文件夹
2. **首次打开前必做** — 在终端执行以下命令（否则 macOS 会提示「已损坏，无法打开」）：

```bash
xattr -cr "/Applications/Agent Smith.app"
```

   或在 Finder 中 **右键 → 打开**（仅首次需要）。这不是应用坏了，而是系统对未公证下载文件的拦截。

3. 打开 Agent Smith
4. 在左侧选择 Claude Code 或 Hermes
5. 点击"New Profile"创建你的第一个配置预设
6. 填入 API Key、模型名称等信息，保存
7. 点击预设左侧的圆圈"启用"，配置立即生效
8. 切换到"Token"页面查看用量统计

---

*Agent Smith — 让 AI 工具的配置管理像切换浏览器标签一样简单。*
