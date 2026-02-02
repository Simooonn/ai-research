# Clawdbot (OpenClaw) 上手指南

> 日期: 2026-02-01
> 前置条件: Node.js >= 22, 消息平台账号, AI 模型 API Key 或订阅

---

## 概述

本指南提供三种完全自包含的部署方案，帮助你从零开始搭建基于 OpenClaw 的智能消息代理（Clawdbot）。无论你是本地实验、追求安全隔离，还是需要 24/7 云端运行，都能找到对应的完整流水线。

**预期成果**：完成本指南后，你将拥有一个连接到至少一个消息渠道（如 Telegram、WhatsApp、Slack 等）的 AI 代理，能够接收消息、调用 AI 模型（推荐 Claude Opus 4.5）生成回复，并具备基本的安全防护。

---

## 环境要求

| 项目 | 要求 |
|------|------|
| 操作系统 | macOS / Linux / Windows (WSL2) |
| 运行时 | Node.js >= 22（推荐使用 [nvm](https://github.com/nvm-sh/nvm) 管理版本） |
| 包管理 | npm / pnpm / bun（任选其一） |
| 可选 | Docker >= 24, Tailscale |
| AI 模型 | Anthropic API Key 或 Pro/Max 订阅（推荐）；OpenAI API Key；或本地 LLM ([Ollama](https://ollama.ai)) |

验证 Node 版本：

```bash
node --version
# 应输出 v22.x.x 或更高
```

如果版本不满足，使用 nvm 安装：

```bash
nvm install 22
nvm use 22
```

---

## 方案 A: 本地快速安装（推荐新手）

本方案适用于个人开发机上的快速体验。所有操作在本地完成，Gateway 仅监听 loopback 地址，无需额外网络配置。

### A-1: 环境准备

1. 确保 Node.js >= 22 已安装：

```bash
node --version   # 确认 v22+
npm --version    # 确认 npm 可用
```

2. （可选）安装 pnpm 以获得更快的依赖解析：

```bash
npm install -g pnpm
```

3. 准备好你的 AI 模型凭证：
   - **Anthropic**（推荐）：前往 [console.anthropic.com](https://console.anthropic.com) 创建 API Key，或确认你拥有 Pro/Max 订阅
   - **OpenAI**：前往 [platform.openai.com](https://platform.openai.com) 创建 API Key
   - **本地 LLM**：安装 [Ollama](https://ollama.ai) 并拉取模型（如 `ollama pull llama3`）

### A-2: 安装 OpenClaw

使用 npm 全局安装：

```bash
npm install -g openclaw@latest
```

或使用 pnpm：

```bash
pnpm add -g openclaw@latest
```

验证安装：

```bash
openclaw --version
```

### A-3: 运行引导向导

引导向导将交互式地完成初始配置，包括 AI 模型连接、Gateway 守护进程安装等：

```bash
openclaw onboard --install-daemon
```

向导会依次询问：

1. **AI 模型提供商**：选择 Anthropic / OpenAI / Ollama
2. **API Key 或认证方式**：输入你的凭证
3. **推荐模型**：默认选择 Claude Opus 4.5（如使用 Anthropic）
4. **Gateway 端口**：默认 `18789`，一般无需修改
5. **守护进程安装**：确认安装，使 Gateway 开机自启

完成后，Gateway 将运行在 `ws://127.0.0.1:18789`。

验证 Gateway 状态：

```bash
openclaw doctor
```

所有检查项应显示通过。如有失败项，根据提示逐一修复。

### A-4: 连接消息渠道（以 Telegram 为例）

OpenClaw 支持 15+ 消息渠道。这里以 Telegram 为例演示完整流程。

**步骤 1：创建 Telegram Bot**

1. 在 Telegram 中搜索 [@BotFather](https://t.me/BotFather) 并启动对话
2. 发送 `/newbot`，按提示设置 Bot 名称和用户名
3. 记录返回的 **Bot Token**（格式类似 `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`）

**步骤 2：在 OpenClaw 中配置 Telegram 渠道**

引导向导可能已经包含了渠道配置步骤。如果没有，手动编辑配置文件（通常位于 `~/.openclaw/config.yaml` 或项目根目录），添加 Telegram 渠道：

```yaml
channels:
  telegram:
    enabled: true
    token: "YOUR_TELEGRAM_BOT_TOKEN"
    dmPolicy: "pairing"   # 推荐：未知发送者需要配对码
```

**步骤 3：重启 Gateway**

```bash
openclaw gateway --port 18789 --verbose
```

**步骤 4：测试连接**

在 Telegram 中向你的 Bot 发送一条消息。由于默认 `dmPolicy="pairing"`，Bot 会返回一个配对码。在终端中批准：

```bash
openclaw pairing approve telegram <配对码>
```

批准后，再次发送消息，Bot 应正常回复。

**其他渠道的快速说明**：

| 渠道 | 底层库 | 关键配置项 |
|------|--------|-----------|
| WhatsApp | Baileys | 扫描 QR 码登录 |
| Slack | Bolt | Bot Token + App-Level Token |
| Discord | discord.js | Bot Token + Gateway Intents |
| Google Chat | Chat API | Service Account JSON |
| Signal | signal-cli | 手机号注册 |
| iMessage | imsg | 仅 macOS，需要 Apple ID |

### A-5: 基础使用与测试

**通过 CLI 直接对话**：

```bash
openclaw agent --message "你好，请介绍一下你自己" --thinking high
```

**通过 CLI 发送消息到渠道**：

```bash
openclaw message send --to +1234567890 --message "Hello from OpenClaw!"
```

**查看 Gateway 日志**：

```bash
openclaw gateway --verbose
```

**健康检查**：

```bash
openclaw doctor
```

### A-6: 安全加固

即使是本地安装，也建议执行以下安全措施：

1. **保持 DM pairing 策略**：不要轻易改为 `dmPolicy="open"`

```yaml
channels:
  telegram:
    dmPolicy: "pairing"   # 默认值，保持不变
```

2. **确保 Gateway 仅监听 loopback**：

```yaml
gateway:
  bind: "127.0.0.1"   # 不要改为 0.0.0.0
  port: 18789
```

3. **定期更新**：

```bash
openclaw update --channel stable
```

4. **运行安全检查**：

```bash
openclaw doctor
```

---

## 方案 B: Docker 安全部署

本方案适用于需要更强安全隔离的场景。通过 Docker 容器化运行 OpenClaw，实现非 root 用户、只读文件系统等安全特性。

### B-1: 环境准备

1. 安装 Docker（>= 24）：

```bash
# macOS
brew install --cask docker

# Ubuntu/Debian
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
# 注销并重新登录使 docker 组生效
```

2. 验证 Docker：

```bash
docker --version
docker run hello-world
```

3. 准备 AI 模型 API Key（同方案 A-1 步骤 3）。

### B-2: Docker 安装与配置

创建项目目录和配置文件：

```bash
mkdir -p ~/openclaw-docker && cd ~/openclaw-docker
```

创建 `docker-compose.yml`：

```yaml
version: "3.8"

services:
  openclaw:
    image: openclaw/openclaw:latest
    container_name: openclaw
    restart: unless-stopped
    user: "1000:1000"          # 非 root 用户
    read_only: true            # 只读文件系统
    tmpfs:
      - /tmp
    ports:
      - "127.0.0.1:18789:18789"   # 仅 loopback
    volumes:
      - ./config:/home/openclaw/.openclaw:rw   # 配置持久化
      - ./data:/home/openclaw/data:rw          # 数据持久化
    environment:
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      # 或者使用 OpenAI:
      # - OPENAI_API_KEY=${OPENAI_API_KEY}
    security_opt:
      - no-new-privileges:true
```

创建 `.env` 文件（**不要提交到版本控制**）：

```bash
ANTHROPIC_API_KEY=sk-ant-xxxxx
```

创建必要目录：

```bash
mkdir -p config data
```

### B-3: 运行 OpenClaw 容器

```bash
cd ~/openclaw-docker
docker compose up -d
```

运行引导向导（在容器内执行）：

```bash
docker compose exec openclaw openclaw onboard --install-daemon
```

验证运行状态：

```bash
docker compose logs -f openclaw
docker compose exec openclaw openclaw doctor
```

### B-4: 连接消息渠道

在容器内配置渠道。编辑 `~/openclaw-docker/config/config.yaml`，添加渠道配置（格式同方案 A-4）。

然后重启容器使配置生效：

```bash
docker compose restart openclaw
```

批准配对请求：

```bash
docker compose exec openclaw openclaw pairing approve telegram <配对码>
```

### B-5: 验证测试

```bash
# 健康检查
docker compose exec openclaw openclaw doctor

# 查看实时日志
docker compose logs -f openclaw

# 通过 CLI 测试对话
docker compose exec openclaw openclaw agent --message "测试消息" --thinking high
```

确保容器在异常退出后自动重启：

```bash
docker compose restart openclaw
docker compose ps   # 确认状态为 running
```

详细 Docker 部署文档参见: [docs.openclaw.ai/install/docker](https://docs.openclaw.ai/install/docker)

---

## 方案 C: 云服务器部署（24/7 运行）

本方案适用于生产环境或需要 7x24 小时不间断运行的场景。以 DigitalOcean 为主要示例，同时说明其他云平台的差异。

### C-1: 选择云服务商

| 平台 | 特点 | 最低配置建议 | 预估月费 |
|------|------|-------------|---------|
| **DigitalOcean** | 1-Click 部署镜像，安全加固 | 2 vCPU / 4 GB RAM | ~$24 |
| **Hetzner Cloud** | 欧洲数据驻留，成本最低 | CX22 (2 vCPU / 4 GB) | ~EUR 5 |
| **AWS EC2** | Pulumi + Tailscale 集成 | t3.small | ~$15 |
| **Vultr** | Docker Compose + 交互式向导 | 2 vCPU / 4 GB | ~$24 |
| **Cloudflare Workers** | 实验性，通过 moltworker 项目 | - | 按用量 |

### C-2: 创建服务器实例

以 **DigitalOcean** 为例（其他平台流程类似）：

1. 登录 [DigitalOcean](https://www.digitalocean.com)
2. 选择 **Marketplace** > 搜索 **OpenClaw** > 使用 1-Click 部署镜像（如可用）
3. 或创建普通 Ubuntu 22.04+ Droplet，选择 2 vCPU / 4 GB RAM
4. 配置 SSH Key 登录（**不要**使用密码登录）
5. 创建完成后记录服务器 IP

**首次登录并初始化**：

```bash
ssh root@YOUR_SERVER_IP

# 创建非 root 用户
adduser openclaw
usermod -aG sudo openclaw
su - openclaw

# 安装 Node.js >= 22
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证
node --version
```

### C-3: 安装与配置 OpenClaw

```bash
# 安装 OpenClaw
sudo npm install -g openclaw@latest

# 运行引导向导
openclaw onboard --install-daemon
```

按照向导提示完成 AI 模型配置和 Gateway 初始化（同方案 A-3）。

### C-4: 配置 Tailscale 远程访问

**强烈建议**：不要将 Gateway 端口直接暴露在公网。使用 Tailscale 建立安全的远程访问通道。

**安装 Tailscale**：

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
# 按照终端输出的链接完成设备授权
```

**配置 OpenClaw 使用 Tailscale**：

编辑 `~/.openclaw/config.yaml`：

```yaml
gateway:
  bind: "127.0.0.1"         # 必须保持 loopback
  port: 18789
  tailscale:
    mode: "serve"            # tailnet-only HTTPS（推荐）
    # mode: "funnel"         # 公网 HTTPS（需要密码认证）
```

三种 Tailscale 模式说明：

| 模式 | 访问范围 | 适用场景 |
|------|---------|---------|
| `off` | 仅 localhost | 纯本地使用 |
| `serve` | Tailnet 内 HTTPS | 推荐，团队内部访问 |
| `funnel` | 公网 HTTPS | 需要外部 Webhook 回调时使用，**需配置密码认证** |

### C-5: 连接消息渠道

与方案 A-4 相同，编辑 `~/.openclaw/config.yaml` 添加渠道配置，然后重启 Gateway。

```bash
openclaw gateway --port 18789 --verbose
```

### C-6: 设置系统服务（持久运行）

如果 `openclaw onboard --install-daemon` 已经安装了守护进程，Gateway 应该已作为系统服务运行。手动确认：

```bash
# 检查服务状态
systemctl --user status openclaw-gateway

# 如果未安装为服务，创建 systemd 服务文件
sudo tee /etc/systemd/system/openclaw.service > /dev/null <<'EOF'
[Unit]
Description=OpenClaw Gateway
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=openclaw
ExecStart=/usr/bin/openclaw gateway --port 18789
Restart=always
RestartSec=10
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable openclaw
sudo systemctl start openclaw
```

验证服务运行：

```bash
sudo systemctl status openclaw
openclaw doctor
```

### C-7: 安全加固与监控

**防火墙配置**（仅允许 SSH 和 Tailscale）：

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow in on tailscale0   # 允许 Tailscale 流量
sudo ufw enable
```

**安全检查清单**：

- [ ] Gateway `bind` 设置为 `127.0.0.1`（**不是** `0.0.0.0`）
- [ ] DM 策略为 `pairing`（不是 `open`）
- [ ] API Key 不在公开的配置文件或仓库中
- [ ] 使用非 root 用户运行 OpenClaw
- [ ] SSH 使用密钥登录，已禁用密码登录
- [ ] 防火墙已启用且规则正确
- [ ] 定期执行 `openclaw doctor` 和 `openclaw update --channel stable`

**三大安全风险及缓解措施**：

| 风险 | 描述 | 缓解措施 |
|------|------|---------|
| **Root** | 主机被入侵 | Docker 沙箱、非 root 用户、只读文件系统 |
| **Agency** | AI 执行意外操作 | DM pairing 策略、权限白名单 |
| **Keys** | 凭证泄露 | 环境变量注入、.env 不入库、定期轮换 |

> **警告**：据统计已有超过 1100 个 OpenClaw 实例因配置不当暴露在公网。请务必遵循上述安全加固步骤。

---

## 技能系统使用

OpenClaw 的技能系统是其核心扩展机制，遵循 Anthropic Agent Skill 规范。

### 技能类型

| 类型 | 说明 | 来源 |
|------|------|------|
| **内置技能** | 随 OpenClaw 安装，开箱即用 | 核心代码 |
| **托管技能** | 从 ClawHub 注册表安装 | [github.com/openclaw/clawhub](https://github.com/openclaw/clawhub) |
| **工作区技能** | 项目本地自定义技能 | 项目目录下 `.openclaw/skills/` |

### 安装托管技能

```bash
# 从 ClawHub 搜索技能
openclaw skill search <关键词>

# 安装技能
openclaw skill install <技能名>
```

### 社区技能资源

- **ClawHub 官方注册表**: [github.com/openclaw/clawhub](https://github.com/openclaw/clawhub)
- **Awesome OpenClaw Skills**: [github.com/VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills)

### 创建自定义技能

在项目目录中创建 `.openclaw/skills/` 文件夹，按 Anthropic Agent Skill 规范编写技能定义文件即可被 OpenClaw 自动识别加载。

---

## 常见问题排查

### Gateway 无法启动

```bash
# 运行诊断
openclaw doctor

# 检查端口是否被占用
lsof -i :18789

# 以 verbose 模式启动查看详细日志
openclaw gateway --port 18789 --verbose
```

### 消息渠道连接失败

1. 确认渠道 Token/凭证正确无误
2. 检查网络连接（部分渠道如 Telegram 需要能访问外网）
3. 查看 Gateway 日志中的具体错误信息
4. 运行 `openclaw doctor` 查看渠道状态

### 配对码不生效

```bash
# 列出待批准的配对请求
openclaw pairing list

# 手动批准
openclaw pairing approve <channel> <code>
```

### AI 模型响应异常

1. 确认 API Key 有效且有余额
2. 检查模型名称配置是否正确
3. 如使用本地 LLM (Ollama)，确认 Ollama 服务正在运行：`ollama list`

### Docker 容器权限问题

```bash
# 确认挂载目录权限
ls -la ~/openclaw-docker/config/
ls -la ~/openclaw-docker/data/

# 修正权限（UID 1000 对应容器内用户）
sudo chown -R 1000:1000 ~/openclaw-docker/config ~/openclaw-docker/data
```

### 更新后功能异常

```bash
# 回退到稳定版
openclaw update --channel stable

# 清理缓存后重启
openclaw gateway --port 18789 --verbose
```

---

## 进阶配置

### 多代理路由

OpenClaw 支持将不同渠道或用户的消息路由到不同的 AI 代理，实现多角色/多用途的消息处理。在 `config.yaml` 中配置路由规则：

```yaml
agents:
  support-bot:
    model: "claude-opus-4-5-20251101"
    skills: ["customer-support"]
  dev-bot:
    model: "claude-opus-4-5-20251101"
    skills: ["coding-assistant"]

routing:
  rules:
    - channel: "slack"
      workspace: "support"
      agent: "support-bot"
    - channel: "telegram"
      agent: "dev-bot"
```

### 语音唤醒

部分渠道支持语音消息处理。OpenClaw 可集成语音转文字服务，实现语音唤醒和语音交互。

### Live Canvas

OpenClaw 支持在支持富媒体的渠道（如 WebChat）中使用 Live Canvas 功能，实时渲染代码、图表等交互内容。

### 定时任务与自动化

通过技能系统或外部 cron 配合 CLI 实现定时任务：

```bash
# 示例：每天早上 9 点发送日报摘要
# 添加到 crontab
crontab -e

# 添加如下行
0 9 * * * /usr/bin/openclaw agent --message "生成今日工作摘要并发送到 Slack #general" --thinking high
```

---

## 参考链接

| 资源 | 链接 |
|------|------|
| OpenClaw 官方文档 | [docs.openclaw.ai](https://docs.openclaw.ai) |
| GitHub 仓库 | [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw) |
| ClawHub 技能注册表 | [github.com/openclaw/clawhub](https://github.com/openclaw/clawhub) |
| Awesome OpenClaw Skills | [github.com/VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills) |
| Docker 部署文档 | [docs.openclaw.ai/install/docker](https://docs.openclaw.ai/install/docker) |
| Tailscale 官网 | [tailscale.com](https://tailscale.com) |
| Node.js 官网 | [nodejs.org](https://nodejs.org) |
| Anthropic Console | [console.anthropic.com](https://console.anthropic.com) |

---

## 从源码构建（开发者）

如果你需要参与 OpenClaw 开发或使用最新未发布的功能：

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm ui:build
pnpm build
pnpm openclaw onboard --install-daemon

# 开发模式（文件变更自动重启）
pnpm gateway:watch
```

---

## CLI 命令速查

```bash
openclaw onboard --install-daemon       # 引导安装 + 守护进程
openclaw gateway --port 18789 --verbose # 启动网关（详细日志）
openclaw doctor                         # 健康检查
openclaw agent --message "..." --thinking high  # CLI 对话
openclaw message send --to <目标> --message "..." # 发送消息
openclaw pairing approve <channel> <code>        # 批准配对
openclaw update --channel stable|beta|dev        # 更新版本
```
