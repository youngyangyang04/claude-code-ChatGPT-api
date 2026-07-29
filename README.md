# 国内如何使用 Claude Code？Claude Opus 5 API 接入完整教程

> 国内使用 Claude Code、接入 Claude Opus API 的中文教程。解决 Claude Code 安装、Claude 账号、海外网络、API Key、海外付款以及 `ANTHROPIC_BASE_URL` 配置问题。

[![Claude Code](https://img.shields.io/badge/Claude_Code-CLI-D97757)](https://code.claude.com/docs)
[![Claude Opus 5](https://img.shields.io/badge/Claude-Opus_5-D97757)](https://platform.claude.com/docs/en/release-notes/overview)
[![APIDock](https://img.shields.io/badge/API-APIDock-4F46E5)](https://apidock.ai/)

很多国内开发者想用 Claude Code 写代码，却卡在这些地方：

- Claude 官网和 API 在国内网络环境下不好访问；
- 注册 Claude 账号后遇到风控或封号；
- 没有海外信用卡，无法开通 Claude Pro、Max 或官方 API；
- 不知道 Claude Code 如何配置第三方 API；
- 想使用最新 Claude Opus 5，但不知道模型名和接入方式。

如果你能解决海外网络、账号和付款问题，可以使用 Claude 官方账号或官方 API。

如果你只想尽快把 Claude Code 和 Opus API 跑起来，可以使用支持 Anthropic Messages API 的中转服务。本文以 [APIDock](https://apidock.ai/) 为例：创建独立令牌，配置到 Claude Code，即可调用 Claude Opus 5，不需要提供 Claude 账号密码。

> [注册 APIDock](https://apidock.ai/register) · [查看 Claude Code 一键接入教程](https://apidock.ai/docs/apidock-easy-install)

## Claude Code 是什么

Claude Code 是 Anthropic 推出的终端 AI 编程工具。它不只是聊天窗口，而是一个能在项目中实际工作的 Coding Agent：

- 阅读整个代码仓库；
- 查找调用链和 Bug 根因；
- 跨文件修改代码；
- 执行 Shell 命令；
- 运行测试并继续修复；
- 使用 MCP、Skills 和子 Agent；
- 完成长时间、多步骤的开发任务。

日常使用时，只需要在项目目录执行：

```bash
claude
```

然后直接描述任务，例如：

```text
先阅读这个项目，找到登录接口的完整调用链。
定位为什么刷新 Token 后仍然返回 401，修复并运行相关测试。
```

## 国内使用 Claude Code 的两种方式

| 方式 | 怎么使用 | 适合谁 | 主要门槛 |
| --- | --- | --- | --- |
| **Claude 官方账号 / API** | 登录 Claude 账号，开通 Pro、Max 或官方 API | 需要 Claude 网页端、Projects 和官方完整体验 | 海外网络、账号风控、海外付款 |
| **APIDock API 接入** | 创建独立令牌，配置到 Claude Code | 没有海外卡、账号不稳定、主要用 CLI 写代码 | 按 Token 计费，需要配置 Base URL |

两种方式不是同一个产品：

- Claude Pro / Max 是账号订阅，包含相应套餐额度；
- Claude API 按 Token 单独计费；
- APIDock 令牌也是独立 API 计费，不会获得 Claude 官网账号或历史会话。

想要 Claude 官网完整功能，选择官方账号。只想稳定使用 Claude Code 和 Opus 模型写代码，API 接入更直接。

## Claude Opus 5 API 信息

Claude Opus 5 是当前适合复杂 Agent 编程任务的 Opus 主力模型。

| 项目 | Claude Opus 5 |
| --- | --- |
| API 模型名 | `claude-opus-5` |
| 上下文窗口 | 100 万 Token |
| 最大输出 | 128k Token |
| 官方标准输入价格 | $5 / 百万 Token |
| 官方标准输出价格 | $25 / 百万 Token |
| 适合任务 | 大型重构、复杂排障、长链路 Agent、系统设计 |

模型名要写成：

```text
claude-opus-5
```

不要写成 `claude-5-opus`，也不要自己在后面添加发布日期。模型名错误通常会返回 404。

以上规格和官方价格来自 [Claude Platform 发布说明](https://platform.claude.com/docs/en/release-notes/overview)。通过 APIDock 调用时，实际价格、倍率和模型可用性以 APIDock 控制台当天显示为准。

## 方法一：使用 Claude 官方账号

官方账号适合需要 Claude 网页端、历史会话、Projects、Remote Control 或完整订阅体验的用户。

大致流程：

1. 准备 Anthropic 支持地区的稳定网络环境；
2. 注册并登录 Claude 账号；
3. 开通 Claude Pro 或 Max；
4. 安装 Claude Code；
5. 执行 `claude`，使用 Claude 账号登录。

国内用户常见问题不是 Claude Code 本身，而是账号、网络和支付：

- 登录 IP 和地区频繁变化可能触发风控；
- 国内银行卡可能无法完成订阅；
- 支付环境、账号地区和网络地区不一致可能导致付款失败；
- 账号被封后，Claude Code 中保存的登录状态也无法继续使用。

不要购买多人共享账号，也不要把邮箱密码和验证码交给第三方。即使暂时能登录，后续项目记录和账号控制权也不在自己手里。

## 方法二：通过 APIDock 接入 Claude Opus API

APIDock官网：[apidock.ai](apidock.ai)

这条路不依赖 Claude 官网账号。

原理很简单：

```text
Claude Code
    │  Anthropic Messages API
    ▼
APIDock API Gateway
    │  令牌鉴权、计费、模型路由
    ▼
Claude Opus 5
```

Claude Code 官方支持通过 `ANTHROPIC_BASE_URL` 连接 LLM Gateway，并通过 `ANTHROPIC_AUTH_TOKEN` 使用 Bearer Token 鉴权。

APIDock 提供 Claude 兼容接口、独立令牌和调用明细。国内用户可以使用支付宝或微信完成付款，不需要把 Claude 密码、邮箱验证码或 Cookie 交给平台。

### 第一步：安装 Claude Code

已经安装的可以跳过。

```bash
npm install -g @anthropic-ai/claude-code
claude --version
```

其他安装方式请参考 [Claude Code 官方安装文档](https://code.claude.com/docs/en/setup)。

### 第二步：创建 APIDock 令牌

1. 打开 [APIDock](https://apidock.ai/register) 注册账号；
2. 进入控制台；
3. 打开「令牌管理」；
4. 创建一个独立令牌；
5. 确认模型列表中已经开放 `claude-opus-5`。

令牌相当于账户余额的钥匙。不要截图发群，不要写入前端代码，也不要提交到 GitHub。

### 第三步：一键接入 Claude Code

新手建议直接使用 [APIDock Claude Code 一键接入教程](https://apidock.ai/docs/apidock-easy-install)。

按照页面中的最新命令执行，一键工具会创建 APIDock 专用启动方式和独立配置，避免覆盖本机原来的 Claude Code 官方账号环境。

安装脚本地址和启动参数可能更新，因此本 README 不复制可能过期的下载命令，以 APIDock 当前文档为准。

### 第四步：启动 Opus 5

如果使用手动临时配置，可以在 macOS 或 Linux 终端执行：

```bash
ANTHROPIC_AUTH_TOKEN="你的 APIDock 令牌" \
ANTHROPIC_BASE_URL="https://apidock.ai" \
claude --model claude-opus-5
```

请把 `ANTHROPIC_AUTH_TOKEN` 替换为自己的令牌。若 APIDock 最新接入文档提供了不同的 Base URL，以当前文档为准。

Windows PowerShell：

```powershell
$env:ANTHROPIC_AUTH_TOKEN="你的 APIDock 令牌"
$env:ANTHROPIC_BASE_URL="https://apidock.ai"
claude --model claude-opus-5
```

第一次先用临时环境变量跑通，不要急着修改永久配置。

### 第五步：验证是否接入成功

进入 Claude Code 后执行：

```text
/status
```

重点检查：

- `Anthropic base URL` 是否显示 APIDock；
- 鉴权来源是否为 `ANTHROPIC_AUTH_TOKEN`；
- 当前模型是否为 `claude-opus-5`。

也可以执行：

```text
/model
```

查看或切换当前模型。

最后打开 APIDock 控制台查看调用记录。Claude Code 状态、模型名和 APIDock 调用明细三处一致，才算真正接入成功。

## 永久配置 Claude Code

临时命令跑通后，可以把配置保存到用户级 `~/.claude/settings.json`：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的 APIDock 令牌",
    "ANTHROPIC_BASE_URL": "https://apidock.ai"
  }
}
```

保存后重新打开终端：

```bash
claude --model claude-opus-5
```

配置文件注意事项：

- `~/.claude/settings.json`：当前用户的所有项目生效；
- `.claude/settings.local.json`：仅当前项目生效，应确认它不会被 Git 提交；
- `.claude/settings.json`：可能被团队共享，不要在里面写真实令牌。

如果你同时使用 Claude 官方账号和 APIDock，推荐使用 APIDock 一键工具创建的独立启动命令，避免两套鉴权互相覆盖。

## 使用 curl 测试 Opus API

如果 Claude Code 启动失败，可以先绕开客户端，直接测试令牌和接口。

```bash
curl "https://apidock.ai/v1/messages" \
  -H "Authorization: Bearer 你的 APIDock 令牌" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-opus-5",
    "max_tokens": 64,
    "messages": [
      {
        "role": "user",
        "content": "你好，请只回复 API 已接通"
      }
    ]
  }'
```

如果 APIDock 当前文档提供的 API 地址不是上面的路径，请替换为控制台显示的实际地址。

判断方法：

- curl 能返回、Claude Code 不能：检查 Claude Code 环境变量和本地配置；
- curl 也不能返回：检查 Base URL、令牌、余额和模型权限。

## 常见报错

### 401 Unauthorized

通常是令牌错误或鉴权变量用错。

- 检查令牌是否复制完整；
- 确认令牌没有被删除或禁用；
- 查看 APIDock 余额；
- 使用 Bearer Token 时配置 `ANTHROPIC_AUTH_TOKEN`；
- 不要同时混用官方 Key、中转令牌和多个鉴权变量。

Claude Code 会优先使用 `ANTHROPIC_AUTH_TOKEN`，其次才是 `ANTHROPIC_API_KEY` 和已保存的账号登录。可以通过 `/status` 确认实际生效的凭证。

### 404 Not Found

通常是 Base URL 或模型名错误。

- 模型名使用 `claude-opus-5`；
- 不要写成 `claude-5-opus`；
- 不要给 Base URL 重复添加 `/v1` 或 `/v1/messages`；
- 回到 [APIDock 最新接入文档](https://apidock.ai/docs/apidock-easy-install) 核对地址。

### 400 Bad Request

通常是参数或协议不兼容。

先删除自定义 `temperature`、`top_p`、旧版 thinking 参数等高级配置，只保留 `model`、`max_tokens` 和 `messages`，用最小请求测试。

### 429 Too Many Requests

通常是余额、限流或并发问题。

- 查看余额和调用明细；
- 降低并发数；
- 批量任务增加指数退避；
- 不要失败后立即无限重试。

### 配置后仍然消耗 Claude 官方额度

只设置 `ANTHROPIC_BASE_URL`，但网关令牌没有生效时，本机保存的 Claude 登录仍可能成为当前鉴权来源。

进入 Claude Code 执行 `/status`，确认：

- Base URL 已经切换；
- `ANTHROPIC_AUTH_TOKEN` 已经生效；
- 当前模型是目标 Opus 模型。

### VS Code 中不生效

从桌面图标启动的 VS Code 不一定继承终端环境变量。

可以从已经配置变量的终端执行：

```bash
code .
```

或者在 VS Code 用户设置的 `claudeCode.environmentVariables` 中配置 Base URL 和令牌。不要把真实令牌写进项目共享配置。

## Opus 5、Sonnet 5、Opus 4.8 怎么选

| 任务 | 推荐模型 | 原因 |
| --- | --- | --- |
| 大仓库重构、复杂排障、长时间 Agent | **Opus 5** | 深度推理和长链路任务能力强 |
| 日常写代码、接口开发、补测试 | Sonnet 5 | 速度、能力和成本更均衡 |
| 已经验证稳定的旧工作流 | Opus 4.8 | 行为熟悉，迁移风险较低 |
| 改文案、分类、简单脚本 | Haiku 或更便宜的模型 | 没必要为轻任务使用 Opus |

不要所有任务都固定使用 Opus 5。复杂任务用 Opus，日常任务使用 Sonnet，轻量任务下沉到更便宜的模型，API 成本会更可控。

## 官方 API 和 APIDock 怎么选

| 对比项 | Claude 官方 API | APIDock |
| --- | --- | --- |
| Claude Code | 支持 | 支持 Claude 兼容接口 |
| 模型 | Claude 官方模型 | 以 APIDock 模型列表为准 |
| 国内网络 | 需要自行解决 | 面向国内接入 |
| 付款 | 通常需要海外支付方式 | 支持国内常用付款方式 |
| 计费 | Anthropic 官方价格 | 以 APIDock 后台实时价格为准 |
| 调用明细 | Claude Console | APIDock 控制台 |
| Claude 官网账号 | 需要单独注册 | 不需要提供 Claude 账号 |

如果你已经有稳定的 Claude 官方 API 账号和海外付款方式，直接使用官方 API 即可。

如果你卡在网络、账号、海外卡或封号问题，主要目标只是使用 Claude Code 写代码，可以先用 [APIDock](https://apidock.ai/register) 小额跑通，再决定是否长期使用。

## 使用 API 中转的安全建议

1. **不要一次充值太多**：先跑通真实任务，再按实际用量充值；
2. **不要泄露令牌**：不要提交到 GitHub、前端代码或公开截图；
3. **核对调用明细**：检查模型名、输入输出 Token 和实际扣费；
4. **谨慎上传敏感代码**：使用第三方服务前，确认数据与日志政策；
5. **令牌泄露立即轮换**：删除旧令牌并创建新令牌；
6. **为令牌设置额度**：如果平台支持，限制单个令牌的余额和权限。

## 常见问题

### 国内如何使用 Claude Code？

有稳定海外网络、Claude 账号和付款方式，可以走官方订阅或官方 API。没有海外卡、账号不稳定，或者只想使用 Claude Code CLI，可以通过 APIDock 创建令牌，配置 `ANTHROPIC_BASE_URL` 和 `ANTHROPIC_AUTH_TOKEN` 后使用。

### Claude Code 可以不登录 Claude 账号吗？

可以。使用 API Key 或 LLM Gateway 令牌时，不需要 Claude 官网账号登录。API 用量和 Claude Pro、Max 订阅是两套独立计费体系。

### Claude Code 如何接入 Opus API？

创建支持 Anthropic Messages API 的令牌，把 `ANTHROPIC_BASE_URL` 指向 API 接入地址，把令牌放入 `ANTHROPIC_AUTH_TOKEN`，然后执行 `claude --model claude-opus-5`。

### Claude Opus 5 的 API 模型名是什么？

模型名是 `claude-opus-5`。不要写成 `claude-5-opus`，也不要自行添加日期后缀。

### Claude Code 接入 API 后如何确认模型？

执行 `/status` 检查 Base URL 和鉴权来源，再执行 `/model` 查看当前模型，同时到 API 控制台核对调用明细。

### APIDock 需要提供 Claude 密码吗？

不需要。你使用的是 APIDock 控制台创建的独立令牌。任何要求提供 Claude 密码、邮箱验证码或 Cookie 的渠道都应谨慎对待。

### Claude 账号被封后还能使用 Claude Code 吗？

被封账号本身无法继续登录，也无法恢复历史会话。但 API 网关令牌是独立入口，可以继续在本地 Claude Code 中调用平台支持的 Claude 模型。

## 立即开始

国内使用 Claude Code、接入 Claude Opus 5 API，最短流程是：

1. [注册 APIDock](https://apidock.ai/register)；
2. 在控制台创建独立令牌；
3. 按 [APIDock 一键接入教程](https://apidock.ai/docs/apidock-easy-install) 配置 Claude Code；
4. 使用 `claude --model claude-opus-5` 启动；
5. 通过 `/status` 和 APIDock 调用明细完成验证。

如果本教程对你有帮助，欢迎 Star 本仓库。

---

本仓库为社区教程，与 Anthropic 无官方关联。Claude Code、模型、API 配置和平台价格可能更新，请以 [Claude Code 官方文档](https://code.claude.com/docs) 和 [APIDock 当前接入文档](https://apidock.ai/docs/apidock-easy-install) 为准。
