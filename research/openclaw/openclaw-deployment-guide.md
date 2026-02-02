# OpenClaw 部署与使用完全指南

> 日期：2026-01-31
> 基于 openclaw-deep-research.md 的深入调研扩展

---

## TL;DR

本文是一份面向零基础用户的 OpenClaw（原名 Clawdbot）**完整部署和使用指南**。涵盖硬件选择（不一定要 Mac Mini！）、4 种部署方案（每种都是从头到尾的完整流程）、自定义 API URL 配置（✅ 支持）、通讯平台接入、安全配置等。

**阅读方式**：先看"第一章"选择你的方案，然后直接跳到对应方案的章节，从头跟到尾即可完成部署。

---

## 一、先选方案：我该用哪种部署方式？

### 硬件最低要求（所有方案通用）

| 配置项 | 最低要求 | 推荐配置 | 说明 |
|--------|---------|---------|------|
| **CPU** | 2 核 | 4 核+ | 基本聊天 2 核足够 |
| **内存** | 2 GB RAM | 4 GB+ RAM | 浏览器自动化/多任务需要 4GB+ |
| **存储** | 20 GB | 50 GB+ | 对话历史和工作空间会持续增长 |
| **运行时** | Node.js >= 22 | Node.js LTS 最新版 | 必须 |
| **网络** | 稳定网络连接 | 有线连接更稳定 | 需要连接 AI API 和通讯平台 |

> ⚠️ **关键认知**：OpenClaw 本身**不跑本地大模型**，它通过 API 调用远端的 Claude/GPT。所以对硬件要求很低——它本质上是一个**网关 + Agent 调度器**，不是需要 GPU 的推理引擎。

### Mac Mini M4 是最低要求吗？

**完全不是！** Mac Mini M4 只是因为低功耗、静音、macOS 生态（iMessage）被追捧，不是必须的。

| 原因 | 说明 |
|------|------|
| 💡 **低功耗 24/7 运行** | 功耗极低，适合长期开着当"家庭服务器" |
| 🔇 **静音无风扇** | 放在桌上不会有噪音 |
| 🍎 **macOS 生态** | iMessage 集成**只在 macOS 上可用** |
| 🔥 **社区跟风** | 社交媒体效应，很多人看到别人买就跟着买了 |

### 4 种方案速览——选择你的路线

| 方案 | 适合谁 | 月成本 | 难度 | iMessage | 安全性 |
|------|--------|--------|------|----------|--------|
| [**方案 A：本地安装**](#二方案-a本地安装macos--linux) | 想先试试 / 有 Mac Mini | $0（电费另算） | ⭐⭐ | ✅ macOS 可用 | ⚠️ 需手动加固 |
| [**方案 B：Docker 部署**](#三方案-bdocker-部署任意平台) | 熟悉 Docker / 想要隔离 | $0（电费另算） | ⭐⭐⭐ | ❌ | ✅ 容器隔离 |
| [**方案 C：DigitalOcean 一键部署**](#四方案-cdigitalocean-一键部署) | 不想折腾 / 想要安全 | $6-24/月 | ⭐ | ❌ | ✅✅ 内置加固 |
| [**方案 D：Railway 一键部署**](#五方案-drailway-一键部署) | 零命令行经验 | $5-20/月 | ⭐ | ❌ | ✅ 隔离环境 |

**快速决策：**

```
需要 iMessage？           → 方案 A（必须 macOS）
想要最安全？              → 方案 C（DigitalOcean，内置安全加固）
零命令行经验？            → 方案 D（Railway，全程 Web 界面）
想先在自己电脑试试？      → 方案 A（零成本入门）
熟悉 Docker 想要隔离？    → 方案 B（容器化）
```

### 自定义 API URL（提前回答）

**✅ 所有方案都支持自定义 API URL！** 每个方案的流程中都会包含具体的配置步骤。你可以使用：

- 🔄 第三方 API 代理（降低成本 40-60%）
- 🌐 OpenRouter（聚合数百个模型）
- 🏠 自建 API 中转（完全掌控）
- 🇨🇳 国内 API 中转服务（解决网络访问问题）

---

## 二、方案 A：本地安装（macOS / Linux）

> 适合：想先试试看 / 有 Mac Mini / 需要 iMessage
> 难度：⭐⭐  |  月成本：$0（电费另算）

### A-1. 安装 Node.js

```bash
# 检查是否已安装（需要 >= 22）
node --version

# --- 如果没装或版本太低 ---

# macOS（使用 Homebrew）:
brew install node

# Ubuntu / Debian:
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# Windows（必须先装 WSL）:
wsl --install
# 然后在 WSL 中执行上面的 Linux 命令
```

### A-2. 安装 OpenClaw

```bash
# 方式 1：一行命令安装（推荐）
curl -fsSL https://openclaw.ai/install.sh | bash

# 方式 2：通过 npm 全局安装
npm install -g openclaw@latest

# 方式 3：从源码构建（开发者）
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm run build
```

安装完成后验证：

```bash
openclaw --version
# 应该输出版本号，如 1.x.x
```

### A-3. 配置 AI 模型（选择一种）

这一步决定 OpenClaw 用哪个 AI 大脑。你有 4 种选择：

---

#### 选项 1：使用 Anthropic 官方 API（最简单）

**前置：** 在 https://console.anthropic.com/ 获取 API Key

```bash
# 运行配置向导，选择 Anthropic
openclaw onboard --install-daemon
# 向导中选择：Anthropic → 粘贴 API Key → 选模型
```

向导完成后，配置会自动写入 `~/.openclaw/config.json`，内容类似：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-xxxxxxxxxxxxxxxx"
  },
  "agents": {
    "defaults": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

---

#### 选项 2：使用 Anthropic + 自定义 API URL（国内用户/代理）

**前置：** 你有一个 Anthropic API Key + 一个代理地址（如 `https://your-proxy.com/v1`）

```bash
# 先运行向导完成基本配置
openclaw onboard --install-daemon

# 然后手动编辑配置文件，添加自定义 Base URL
```

编辑 `~/.openclaw/config.json`（或 `~/.openclaw/openclaw.json`）：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://your-proxy.com/v1"
  },
  "agents": {
    "defaults": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

保存后重启：

```bash
openclaw restart
```

---

#### 选项 3：使用 OpenRouter（聚合多家模型，推荐）

**前置：** 在 https://openrouter.ai/ 注册并获取 API Key（以 `sk-or-` 开头）

```bash
# 一行命令配置
openclaw onboard --auth-choice apiKey \
  --token-provider openrouter \
  --token "sk-or-xxxxxxxxxxxxxxxx"
```

或手动编辑 `~/.openclaw/config.json`：

```json
{
  "env": {
    "OPENROUTER_API_KEY": "sk-or-xxxxxxxxxxxxxxxx"
  },
  "agents": {
    "defaults": {
      "model": "openrouter/auto"
    }
  }
}
```

> `openrouter/auto` 会自动选择最优模型，你也可以指定如 `openrouter/anthropic/claude-sonnet-4`。

---

#### 选项 4：使用 OpenAI Compatible（任意第三方 API）

**前置：** 你有一个兼容 OpenAI 格式的 API 服务地址和 Key

编辑 `~/.openclaw/config.json`：

```json
{
  "env": {
    "OPENAI_API_KEY": "sk-xxxxxxxxxxxxxxxx"
  },
  "agents": {
    "defaults": {
      "model": "openai-compatible/claude-sonnet-4-20250514",
      "providers": {
        "openai-compatible": {
          "baseUrl": "https://your-api-service.com/v1",
          "apiKey": "sk-xxxxxxxxxxxxxxxx"
        }
      }
    }
  }
}
```

> 这种方式可以接入几乎任何 API 中转服务，包括国内的各类中转平台。

---

### A-4. 连接通讯平台

```bash
# WhatsApp（会弹出二维码，用手机扫码）
openclaw channels login whatsapp

# Telegram（需要先在 @BotFather 创建 Bot，获取 Token）
openclaw channels login telegram

# Discord（需要在 Discord Developer Portal 创建 Bot）
openclaw channels login discord

# Slack
openclaw channels login slack

# iMessage（仅 macOS，需要授权辅助功能权限）
openclaw channels login imessage
```

### A-5. 安全加固（强烈建议）

```bash
# 运行安全诊断
openclaw doctor

# 启用认证（防止未授权访问）
openclaw security --enable-auth

# 限制网络监听（仅本机访问）
openclaw configure --bind localhost

# 配置 DM Pairing（限制谁能和你的 AI 对话）
openclaw pairing
```

### A-6. 验证并开始使用

```bash
# 检查运行状态
openclaw status

# 查看日志
openclaw logs

# 打开 Web Dashboard（可选）
openclaw dashboard
```

然后在你连接的通讯应用中给 AI 发消息即可！试试发 "Hello, what can you do?"

### A-7. 设置开机自启（24/7 运行）

如果你在配置向导中选了 `--install-daemon`，守护进程已经自动安装。否则：

```bash
# macOS：使用 launchd
openclaw daemon install

# Linux：使用 systemd
openclaw daemon install
```

---

## 三、方案 B：Docker 部署（任意平台）

> 适合：熟悉 Docker / 想要容器隔离 / 在 VPS 上运行
> 难度：⭐⭐⭐  |  月成本：$0（VPS 费用另算）

### B-1. 安装 Docker

```bash
# macOS：安装 Docker Desktop
brew install --cask docker

# Ubuntu / Debian：
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
# 重新登录使 docker 组生效
```

### B-2. 启动容器（选择你的 API 配置）

---

#### 选项 1：使用 Anthropic 官方 API

```bash
docker run -d \
  --name openclaw \
  --restart unless-stopped \
  -e ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxxxxx" \
  -v openclaw_data:/root/.openclaw \
  -p 3000:3000 \
  ghcr.io/openclaw/openclaw:latest
```

---

#### 选项 2：使用 Anthropic + 自定义 API URL（国内用户/代理）

```bash
docker run -d \
  --name openclaw \
  --restart unless-stopped \
  -e ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxxxxx" \
  -e ANTHROPIC_BASE_URL="https://your-proxy.com/v1" \
  -v openclaw_data:/root/.openclaw \
  -p 3000:3000 \
  ghcr.io/openclaw/openclaw:latest
```

---

#### 选项 3：使用 OpenRouter

```bash
docker run -d \
  --name openclaw \
  --restart unless-stopped \
  -e OPENROUTER_API_KEY="sk-or-xxxxxxxxxxxxxxxx" \
  -v openclaw_data:/root/.openclaw \
  -p 3000:3000 \
  ghcr.io/openclaw/openclaw:latest
```

---

#### 选项 4：使用 OpenAI Compatible（任意第三方 API）

```bash
docker run -d \
  --name openclaw \
  --restart unless-stopped \
  -e OPENAI_API_KEY="sk-xxxxxxxxxxxxxxxx" \
  -e OPENAI_BASE_URL="https://your-api-service.com/v1" \
  -v openclaw_data:/root/.openclaw \
  -p 3000:3000 \
  ghcr.io/openclaw/openclaw:latest
```

---

### B-3. 进入容器完成配置

```bash
# 进入容器运行配置向导
docker exec -it openclaw openclaw onboard

# 向导中配置：
# 1. 如果上一步已通过环境变量设了 API Key，这里跳过即可
# 2. 选择通讯平台（WhatsApp / Telegram / Discord 等）
# 3. 配置权限
```

### B-4. 连接通讯平台

```bash
# 在容器内操作
docker exec -it openclaw openclaw channels login whatsapp
docker exec -it openclaw openclaw channels login telegram
docker exec -it openclaw openclaw channels login discord
```

### B-5. 安全加固

```bash
# 安全诊断
docker exec -it openclaw openclaw doctor

# 启用认证
docker exec -it openclaw openclaw security --enable-auth

# 配置 DM Pairing
docker exec -it openclaw openclaw pairing
```

Docker 本身已提供一层隔离，但建议额外加固：

```bash
# 使用更严格的安全选项重新创建容器
docker stop openclaw && docker rm openclaw

docker run -d \
  --name openclaw \
  --restart unless-stopped \
  --security-opt no-new-privileges \
  --read-only \
  --tmpfs /tmp \
  -e ANTHROPIC_API_KEY="your_key" \
  -e ANTHROPIC_BASE_URL="https://your-proxy.com/v1" \
  -v openclaw_data:/root/.openclaw \
  -p 127.0.0.1:3000:3000 \
  ghcr.io/openclaw/openclaw:latest
```

> 注意 `-p 127.0.0.1:3000:3000` 只在本机监听，不暴露到公网。

### B-6. 验证并使用

```bash
# 查看容器状态
docker ps | grep openclaw

# 查看日志
docker logs -f openclaw

# 健康检查
docker exec -it openclaw openclaw status
```

### B-7. 如果在 VPS 上运行（额外步骤）

```bash
# 1. 创建专用用户（不要用 root）
sudo adduser openclaw-user
sudo usermod -aG docker openclaw-user
su - openclaw-user

# 2. 配置防火墙
sudo ufw allow ssh
sudo ufw allow 3000/tcp   # 仅在需要 Web Dashboard 时开放
sudo ufw enable

# 3. 然后执行 B-2 到 B-6 的步骤
```

---

## 四、方案 C：DigitalOcean 一键部署

> 适合：不想折腾 / 想要最安全的方案 / 零运维经验
> 难度：⭐  |  月成本：$6-24/月

### C-1. 注册并创建 Droplet

```
1. 注册 DigitalOcean 账号（https://www.digitalocean.com/）
2. 创建新 Droplet
3. 在 Marketplace 中搜索 "OpenClaw" 一键镜像
4. 选择配置规格：
   - 入门: 2 GB RAM / 1 vCPU / 50 GB SSD → $12/月
   - 推荐: 4 GB RAM / 2 vCPU / 80 GB SSD → $24/月
5. 选择数据中心（离你近的）
6. 添加 SSH Key（如果没有：ssh-keygen -t rsa -b 4096）
7. 点击创建
```

### C-2. 内置安全措施（自动配置，无需手动）

DigitalOcean 一键镜像已自动配置：

| 安全特性 | 说明 |
|----------|------|
| 🔑 **Gateway Token** | 自动生成，通信已认证 |
| 🛡️ **硬化防火墙** | 默认限速，防 DDoS |
| 👤 **非 root 运行** | 以普通用户身份运行 |
| 📦 **Docker 隔离** | 容器化沙箱 |
| 🔒 **DM Pairing** | 默认启用，防止未授权对话 |

### C-3. SSH 登录并配置 AI 模型

```bash
# SSH 登录（用你创建时的 IP）
ssh root@your_droplet_ip
```

---

#### 选项 1：使用 Anthropic 官方 API

```bash
# 运行向导，选 Anthropic
openclaw onboard
# 选择 Anthropic → 粘贴 API Key → 选模型 → 完成
```

---

#### 选项 2：使用 Anthropic + 自定义 API URL

```bash
# 先完成向导
openclaw onboard

# 然后编辑配置文件
nano ~/.openclaw/config.json
```

在 `"env"` 中添加 `ANTHROPIC_BASE_URL`：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://your-proxy.com/v1"
  }
}
```

保存后重启：

```bash
openclaw restart
```

---

#### 选项 3：使用 OpenRouter

```bash
openclaw onboard --auth-choice apiKey \
  --token-provider openrouter \
  --token "sk-or-xxxxxxxxxxxxxxxx"
```

---

#### 选项 4：使用 OpenAI Compatible

```bash
# 先完成向导
openclaw onboard

# 编辑配置
nano ~/.openclaw/config.json
```

```json
{
  "env": {
    "OPENAI_API_KEY": "sk-xxxxxxxxxxxxxxxx"
  },
  "agents": {
    "defaults": {
      "model": "openai-compatible/claude-sonnet-4-20250514",
      "providers": {
        "openai-compatible": {
          "baseUrl": "https://your-api-service.com/v1",
          "apiKey": "sk-xxxxxxxxxxxxxxxx"
        }
      }
    }
  }
}
```

保存后 `openclaw restart`。

---

### C-4. 连接通讯平台

```bash
# WhatsApp
openclaw channels login whatsapp

# Telegram
openclaw channels login telegram

# Discord
openclaw channels login discord
```

### C-5. 验证并使用

```bash
# 状态检查
openclaw status

# 健康诊断
openclaw doctor

# 查看日志
openclaw logs

# 打开 Web Dashboard（浏览器访问 http://your_droplet_ip:3000）
openclaw dashboard
```

在你的通讯应用中发消息给 AI，开始使用！

---

## 五、方案 D：Railway 一键部署

> 适合：完全零命令行经验 / 想要最快上手
> 难度：⭐  |  月成本：$5-20/月

### D-1. 注册并部署

```
1. 注册 Railway 账号（https://railway.app/）
2. 搜索 OpenClaw 模板（或访问社区模板仓库）
3. 点击 "Deploy on Railway"
4. 设置环境变量（在 Railway 的 Variables 面板中）
```

### D-2. 配置环境变量（在 Railway Web 界面中操作）

在 Railway 的 Variables 面板中添加：

---

#### 选项 1：Anthropic 官方 API

```
ANTHROPIC_API_KEY = sk-ant-xxxxxxxxxxxxxxxx
```

---

#### 选项 2：Anthropic + 自定义 API URL

```
ANTHROPIC_API_KEY = sk-ant-xxxxxxxxxxxxxxxx
ANTHROPIC_BASE_URL = https://your-proxy.com/v1
```

---

#### 选项 3：OpenRouter

```
OPENROUTER_API_KEY = sk-or-xxxxxxxxxxxxxxxx
```

---

#### 选项 4：OpenAI Compatible

```
OPENAI_API_KEY = sk-xxxxxxxxxxxxxxxx
OPENAI_BASE_URL = https://your-api-service.com/v1
```

---

### D-3. 完成配置向导

```
1. 部署完成后，Railway 会给你一个 URL
2. 访问 https://your-app.railway.app/setup
3. 输入 Setup Password（在环境变量中设置的 SETUP_PASSWORD）
4. 在 Web 向导中完成：
   - 确认 AI 模型配置
   - 连接通讯平台（扫码/输入 Token）
   - 设置权限
5. 完成！
```

### D-4. 验证并使用

```
1. 访问 https://your-app.railway.app/ → Web Dashboard
2. 在通讯应用中发消息给 AI
3. 查看 Railway Logs 面板确认正常运行
```

---

## 六、Model Provider 完整参考

### 支持的 AI 模型提供商

| Provider | 环境变量 Key | 环境变量 Base URL | 模型格式示例 |
|----------|-------------|------------------|-------------|
| **Anthropic** | `ANTHROPIC_API_KEY` | `ANTHROPIC_BASE_URL`（可选） | `anthropic/claude-sonnet-4-20250514` |
| **OpenAI** | `OPENAI_API_KEY` | `OPENAI_BASE_URL`（可选） | `openai/gpt-4o` |
| **OpenRouter** | `OPENROUTER_API_KEY` | — | `openrouter/auto` 或 `openrouter/anthropic/claude-sonnet-4` |
| **openai-compatible** | 在 providers 中配置 | 在 providers 中配置 | `openai-compatible/model-name` |
| **Google** | `GOOGLE_API_KEY` | — | `google/gemini-2.0-flash` |

### Model Failover（故障自动切换）

在 `config.json` 中配置，主模型挂了自动切换备用：

```json
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-sonnet-4-20250514",
      "fallbackModels": [
        "openai/gpt-4o",
        "google/gemini-2.0-flash"
      ]
    }
  }
}
```

### 配置方式总结

| 配置方式 | 在哪用 | 说明 |
|----------|-------|------|
| **config.json 文件** | 方案 A/B/C 手动编辑 | 永久生效，推荐 |
| **环境变量** | 方案 B（Docker -e）/ 方案 D（Railway Variables） | 容器化和云平台首选 |
| **配置向导** | 方案 A/C（openclaw onboard） | 交互式，适合首次 |
| **CLI 参数** | 所有方案 | 一次性覆盖，测试用 |

---

## 七、日常使用指南（所有方案通用）

### 常用命令

```bash
openclaw status          # 查看运行状态
openclaw doctor          # 健康诊断
openclaw logs            # 查看日志
openclaw config show     # 查看配置
openclaw tui             # 终端交互界面
openclaw dashboard       # 打开 Web Dashboard
openclaw channels        # 查看已连接平台
openclaw memory          # 查看记忆内容
openclaw update          # 更新到最新版
openclaw restart         # 重启服务
```

> Docker 用户在命令前加 `docker exec -it openclaw`，如：`docker exec -it openclaw openclaw status`

### Skills（技能）管理

```bash
openclaw skills list                  # 列出可用技能
openclaw skills enable skill-name     # 启用技能
openclaw skills disable skill-name    # 禁用技能
```

### 与 AI 对话示例

| 场景 | 示例消息 |
|------|---------|
| 📁 文件查找 | "帮我找一下桌面上的 Q4 报告 PDF" |
| 📧 邮件管理 | "检查我的邮箱，有没有重要邮件" |
| 📅 日程管理 | "明天下午 3 点提醒我开会" |
| 💻 终端命令 | "运行 git status 看看代码仓库状态" |
| 🌐 网页研究 | "帮我搜索一下 2026 年最新的 AI 趋势" |
| 📊 数据分析 | "分析一下这个 CSV 文件的数据分布" |
| 🔔 定时任务 | "每天早上 9 点给我发一份新闻摘要" |

### Multi-Agent（多代理路由）

你可以为不同的人/渠道配置不同的 Agent，它们有独立的记忆和权限：

```
家人的 Telegram → Agent A（生活助手，权限较少）
你的 WhatsApp  → Agent B（全功能助手，权限完整）
工作 Slack     → Agent C（工作助手，访问代码仓库）
```

---

## 八、安全清单（所有方案通用）

| 检查项 | 操作 | 重要性 | 适用方案 |
|--------|------|--------|---------|
| ✅ 不要以 root 运行 | 创建专用用户 | 🔴 Critical | A / B / C |
| ✅ 启用认证 | `openclaw security --enable-auth` | 🔴 Critical | A / B |
| ✅ 关闭公网管理端口 | 防火墙 / 绑定 localhost | 🔴 Critical | A / B |
| ✅ 使用 DM Pairing | `openclaw pairing` | 🟠 High | 所有 |
| ✅ Docker 隔离 | 容器化运行 | 🟠 High | B |
| ✅ 审查第三方 Skills | 不要盲目安装 | 🟠 High | 所有 |
| ✅ 限制文件系统访问 | 不给全盘权限 | 🟠 High | A |
| ✅ 定期更新 | `openclaw update` | 🟡 Medium | 所有 |
| ✅ 加密存储 | 敏感数据加密 | 🟡 Medium | 所有 |

> 方案 C（DigitalOcean）大部分安全措施已自动配置。方案 D（Railway）运行在隔离环境中。

---

## 九、费用估算

### AI API 费用（主要开销）

| 模型 | 输入价格 | 输出价格 | 日均估算（轻度） |
|------|---------|---------|-----------------|
| Claude Sonnet 4 | $3/M tokens | $15/M tokens | ~$0.5-2/天 |
| Claude Opus 4.5 | $15/M tokens | $75/M tokens | ~$2-10/天 |
| GPT-4o | $2.5/M tokens | $10/M tokens | ~$0.3-1.5/天 |
| 通过 OpenRouter/代理 | 降低 40-60% | 降低 40-60% | 按比例降低 |

### 总费用估算

| 方案 | 硬件/托管 | API 费用 | 月总计 |
|------|----------|---------|--------|
| **A: 现有电脑** | 电费 $5-15 | $15-60 | **$20-75** |
| **A: Mac Mini M4** | 首月含购机 $502 / 之后 $3 | $15-60 | **首月 $517-562 / 之后 $18-63** |
| **B: Docker (VPS)** | $4-24 | $15-60 | **$19-84** |
| **C: DigitalOcean** | $6-24 | $15-60 | **$21-84** |
| **D: Railway** | $5-20 | $15-60 | **$20-80** |

---

## 十、常见问题 FAQ

### Q1: Windows 可以用吗？

**可以**，通过 WSL 运行，或使用方案 B/C/D（不依赖本地系统）。

### Q2: 国内网络无法访问 Anthropic API？

在你选择的方案中，配置 `ANTHROPIC_BASE_URL` 指向代理地址即可。每个方案的步骤中都有对应说明。

### Q3: 可以同时连接多个通讯平台吗？

**可以！** 所有平台共享同一个 AI 和记忆。

### Q4: 数据存在哪里？

- 对话历史、记忆、文件 → **本地** `~/.openclaw/` 目录（Docker 中在 volume 里）
- AI 推理请求 → 发送到你配置的 API 端点
- 通讯平台消息 → 通过各平台官方协议传输

### Q5: 如何卸载？

```bash
openclaw uninstall
# 或
npm uninstall -g openclaw
# Docker: docker stop openclaw && docker rm openclaw
```

---

## 十一、推荐学习路径

```
第 1 天：选一个方案部署，连接 Telegram，发第一条消息
    ↓
第 2-3 天：探索 Skills，尝试文件操作、网页搜索
    ↓
第 4-5 天：配置自定义 API URL，优化成本
    ↓
第 1-2 周：安全加固，探索高级功能（定时任务、多平台、Multi-Agent）
    ↓
进阶：如果需要 24/7 运行，迁移到 VPS 或 Mac Mini
```

---

## 参考来源

- [OpenClaw 官方 Wiki - Getting Started](https://openclawwiki.com/getting-started.html) - 官方入门指南
- [OpenClaw 官方文档 - Model Providers](https://docs.openclaw.ai/concepts/model-providers) - 模型配置文档
- [OpenRouter - OpenClaw Integration](https://openrouter.ai/docs/guides/guides/openclaw-integration) - OpenRouter 集成指南
- [APIYI - OpenClaw API Proxy Tutorial](https://help.apiyi.com/en/openclaw-api-proxy-configuration-tutorial-en.html) - 第三方 API 代理配置教程
- [DigitalOcean - How to Run OpenClaw](https://www.digitalocean.com/community/tutorials/how-to-run-openclaw) - DigitalOcean 一键部署教程
- [DigitalOcean - OpenClaw Quickstart Guide](https://www.digitalocean.com/community/tutorials/openclaw-quickstart-guide) - 云端部署快速入门
- [Dev.to - You Don't Need a Mac Mini](https://dev.to/sivarampg/you-dont-need-a-mac-mini-to-run-clawdbot-heres-how-to-run-it-anywhere-217l) - 硬件选择分析
- [Dev.to - Moltworker Complete Guide](https://dev.to/sienna/moltworker-complete-guide-2026-running-personal-ai-agents-on-cloudflare-without-hardware-4a99) - Cloudflare 部署方案
- [Discord - Custom Anthropic Base URL](https://www.answeroverflow.com/m/1465513231467417642) - 社区自定义 API URL 讨论
- [Beebom - Setup OpenClaw on Mac Mini](https://beebom.com/how-to-set-up-openclaw-on-mac-mini/) - Mac Mini 部署教程
- [DataCamp - OpenClaw Tutorial](https://www.datacamp.com/tutorial/openclaw-tutorial) - 从 WhatsApp 控制电脑教程
- [GrowthJockey - OpenClaw Guide](https://www.growthjockey.com/blogs/openclaw) - 完整安装和架构指南
