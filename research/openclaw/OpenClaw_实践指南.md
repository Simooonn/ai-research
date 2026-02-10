# OpenClaw 使用技巧与实践指南

> **定位：** 面向已有初步使用经验的用户，快速掌握实战技巧和优化方法
> **更新时间：** 2026-02
> **预计阅读时间：** 15 分钟

---

## 一、OpenClaw 核心定位

OpenClaw 是一个**自托管 AI 助手网关**，将多个聊天应用（WhatsApp、Telegram、Discord、iMessage 等）连接到 AI 编码代理。

### 核心特性
- **多渠道统一控制** - 单一 Gateway 同时服务多个平台
- **本地优先架构** - 数据完全自主可控，WebSocket 本地通信
- **代理原生设计** - 支持工具调用、会话管理、内存存储、多代理路由
- **设备集成** - macOS 菜单栏应用、iOS/Android 移动端、语音控制
- **插件扩展** - 支持 Cron 定时任务、Webhook、Gmail、浏览器控制

> **适用人群：** 希望拥有 24/7 可用的个人 AI 助手，同时保持数据控制权的开发者和高级用户

---

## 二、安装配置

### 2.1 环境要求

| 项目 | 要求 |
|------|------|
| **Node.js** | ≥ 22 |
| **操作系统** | macOS / Linux / Windows (WSL2) |
| **构建工具** | pnpm (仅从源码安装时) |

### 2.2 推荐安装方法（自动化脚本）

**macOS / Linux / WSL2:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows PowerShell:**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

> **优势：** 自动处理 Node 检测、安装和引导配置，跳过向导使用 `--no-onboard` 参数

### 2.3 手动安装（npm 方式）

适用于已安装 Node 22+ 的环境：

```bash
# 安装 OpenClaw
npm install -g openclaw@latest

# 引导配置（安装守护进程）
openclaw onboard --install-daemon

# 如遇 sharp 构建错误（macOS Homebrew libvips 冲突）
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

### 2.4 验证安装

```bash
# 诊断配置问题
openclaw doctor

# 检查网关健康状态
openclaw status

# 启动浏览器控制面板
openclaw dashboard
```

### 2.5 关键环境变量

```bash
# 自定义安装路径
export OPENCLAW_HOME=/custom/path

# 可变状态存储目录
export OPENCLAW_STATE_DIR=/data/openclaw

# 配置文件位置
export OPENCLAW_CONFIG_PATH=/etc/openclaw/config.yaml
```

> **PATH 问题修复：** 如果 `openclaw` 命令不可用，检查 npm 全局 bin 目录是否在 `$PATH` 中，通常位于 `~/.npm-global/bin` 或 `$(npm prefix -g)/bin`

---

## 三、核心功能详解

### 3.1 渠道连接管理

```bash
# 登录渠道（交互式向导）
openclaw channels login

# 查看所有渠道状态
openclaw channels status

# 探测渠道连接健康状态
openclaw channels status --probe
```

**支持的渠道：**
- **Telegram** - 功能最全（反应、语音笔记）
- **Discord** - 公会和频道级访问控制
- **WhatsApp** - 基于 Webhook 配置
- **Slack** - Bot 和 App Token 认证
- **iMessage** - 通过 BlueBubbles 集成
- **Signal / Teams / Matrix** - 插件支持

### 3.2 AI 模型配置

在 `~/.openclaw/openclaw.json` 中配置模型：

```json
{
  "models": {
    "primary": {
      "provider": "anthropic",
      "model": "claude-opus-4-6",
      "temperature": 0.7,
      "maxTokens": 4096
    },
    "fallback": {
      "provider": "openai",
      "model": "gpt-4-turbo"
    }
  }
}
```

**模型选择建议：**
- **Anthropic Claude** - 复杂推理和编码任务（推荐）
- **OpenAI GPT** - 通用性能均衡
- **Google Gemini** - 提供免费额度
- **DeepSeek** - 成本优化选择（$0.55/M tokens）

### 3.3 多代理配置

创建专用代理处理不同任务：

```bash
# 创建新代理
openclaw agents create --name dev-assistant --model claude-opus-4-6

# 设置代理工作目录
openclaw agents configure dev-assistant --workspace ~/projects

# 查看所有代理
openclaw agents list
```

**代理个性化配置：** 在 `~/.openclaw/agents/<agent-name>/SOUL.md` 中定义代理身份和行为

### 3.4 安全和权限管理

```bash
# 存储敏感信息到 .env
echo "ANTHROPIC_API_KEY=sk-ant-xxx" >> ~/.openclaw/.env

# 在配置中引用环境变量
# config: apiKey: ${ANTHROPIC_API_KEY}

# 设置文件权限
chmod 600 ~/.openclaw/.env
```

> **安全原则：** OpenClaw 不记录或传输 API 密钥，所有敏感数据本地存储

---

## 四、实战案例

### 4.1 个人生活自动化

**场景一：晨间简报**
```
设置每天 7:00 发送天气、日历事件和待办事项摘要到 Telegram
```

**实现方式：**
```bash
openclaw cron add "0 7 * * *" "生成今日简报：天气、日程、待办，发送到我的 Telegram"
```

**场景二：智能购物清单**
```
从聊天消息中自动提取购物项目，维护共享清单
```

**Prompt 示例：**
```
监听包含"买"或"购"的消息，提取商品名称并添加到 ~/shopping-list.md
```

### 4.2 开发运维自动化

**场景三：服务器监控**
```bash
# 每 5 分钟检查服务器健康状态
openclaw cron add "*/5 * * * *" "检查服务器 CPU、内存、磁盘使用率，超过阈值发送告警"
```

**场景四：CI/CD 通知**
```bash
# 监听 GitHub Webhook，推送构建结果
openclaw webhooks create --url /github --action "解析构建状态并发送到 Discord #dev 频道"
```

**场景五：PR 摘要生成**
```
每天 18:00 总结当天所有 Pull Request，发送到 Slack
```

### 4.3 内容创作工作流

**场景六：多平台内容分发**
```
将 Twitter 长文自动改写成 LinkedIn 版本和小红书版本
```

**场景七：会议记录处理**
```
上传会议录音 → 转录 → 提取行动项 → 生成会议纪要 → 发送给参会者
```

### 4.4 企业应用场景

**场景八：客户支持自动化**
```
监控支持邮箱，自动分类问题类型，生成回复草稿
```

**场景九：品牌监控**
```
每小时搜索品牌提及，分析情感倾向，汇总报告
```

---

## 五、性能优化技巧

### 5.1 延迟优化（目标：响应时间 <1.5s）

#### **技巧 1：禁用冗长思考模式**

```json
// ~/.openclaw/openclaw.json
{
  "thinkingDefault": "minimal"
}
```

**效果：** 响应时间从 ~2.2s 降至 ~1.1s
**权衡：** 复杂推理任务可靠性略降

#### **技巧 2：减少 WebSocket 增量节流**

```bash
export OPENCLAW_WS_DELTA_THROTTLE_MS=20
```

**效果：** Token 流速提升 7 倍（150ms → 20ms）
**适用：** 语音消费场景

#### **技巧 3：启用 Anthropic Prompt 缓存**

```json
{
  "models": {
    "primary": {
      "provider": "anthropic",
      "promptCaching": true
    }
  }
}
```

**要求：** OpenClaw v2026.2.0+
**效果：** 成本降低 90%，响应速度提升

#### **技巧 4：智能模型路由**

```json
{
  "routing": {
    "simple": "claude-haiku",
    "subagents": "deepseek-chat",
    "main": "claude-opus-4-6"
  }
}
```

**成本对比：**
- Opus: $15/M tokens
- DeepSeek: $0.55/M tokens（节省 96%）

#### **技巧 5：减少上下文窗口（本地模型）**

```json
{
  "models": {
    "local": {
      "contextWindow": 4096  // 从 8192 降低
    }
  }
}
```

**效果：** MacBook M2 处理速度从 3.2 → 8 tokens/s

### 5.2 成本优化（实战案例：$347 → $68/月）

#### **策略 1：定期会话重置（40-60% 节省）**

```bash
# 每个独立任务完成后重置
openclaw sessions reset
```

**原因：** 防止上下文累积至 150K tokens

#### **策略 2：内存配置优化**

| 使用场景 | 推荐内存 |
|---------|---------|
| 个人使用 | 4GB |
| 团队协作 | 8GB |
| 生产环境 | 16GB |

**诊断命令：**
```bash
docker stats openclaw-gateway
```

#### **策略 3：上下文窗口限制（20-40% 节省）**

```json
{
  "context": {
    "maxTokens": 8000,
    "pruningMode": "sliding",
    "ttl": 3600
  }
}
```

#### **策略 4：禁用不必要的 Skills（10-15% 节省）**

```bash
# 列出所有 Skills
openclaw skills list

# 禁用未使用的 Skills
openclaw skills disable <skill-name>
```

### 5.3 综合优化效果

| 优化项 | 效果 | 实施难度 |
|-------|------|---------|
| 智能模型路由 | 50-80% 成本节省 | ⭐ 简单 |
| 定期会话重置 | 40-60% 成本节省 | ⭐ 简单 |
| 禁用冗长思考 | 50% 延迟降低 | ⭐ 简单 |
| Prompt 缓存 | 90% 成本 + 速度提升 | ⭐⭐ 中等 |
| 内存优化 | 避免崩溃和卡顿 | ⭐⭐ 中等 |

---

## 六、常见问题与解决方案

### 6.1 快速诊断命令

按顺序执行以下命令定位问题：

```bash
openclaw status              # 基础状态检查
openclaw status --all        # 完整报告
openclaw gateway probe       # 网关可达性
openclaw gateway status      # 网关运行状态
openclaw doctor              # 配置问题诊断
openclaw channels status --probe  # 渠道连接探测
openclaw logs --follow       # 实时日志监控
```

### 6.2 常见问题速查表

| 问题现象 | 诊断命令 | 典型错误信号 | 解决方案 |
|---------|---------|-------------|---------|
| **机器人无回复** | `openclaw channels status --probe` | "drop guild message (mention required)" | 检查提及设置、配对状态 |
| **UI 连接失败** | `openclaw gateway status` | "device identity required" | 验证 Token、切换认证模式 |
| **网关无法启动** | `openclaw doctor` | "Gateway start blocked" | 设置 `gateway.mode=local` |
| **消息流中断** | `openclaw channels status --probe` | "missing_scope" / "403" | 检查渠道权限和令牌 |
| **定时任务不执行** | `openclaw cron runs --id <jobId>` | "scheduler disabled" | 启用 cron 并检查活跃时间 |
| **浏览器工具失败** | `openclaw browser status` | "Failed to start Chrome CDP" | 验证 Chrome 可执行路径 |
| **内存溢出崩溃** | `docker logs openclaw-gateway` | "FATAL ERROR: Reached heap limit" | 增加内存限制或重置会话 |

### 6.3 关键检查点

**正常状态标志：**
```bash
$ openclaw gateway status
Runtime: running  ✓
RPC probe: ok     ✓

$ openclaw channels status
Telegram: connected  ✓
Discord: ready       ✓
```

### 6.4 PATH 问题排查

```bash
# 检查 npm 全局 bin 目录
npm prefix -g

# 添加到 PATH（添加到 ~/.zshrc 或 ~/.bashrc）
export PATH="$(npm prefix -g)/bin:$PATH"
```

---

## 七、高级使用技巧

### 7.1 结构化 Prompt 模式

**最佳实践模板：**
```
[目标] + [输入] + [输出格式]

示例：
总结过去 20 封支持邮件，按主题分组，返回 Markdown 表格
```

**关键原则：**
- 明确目标和约束
- 拆解复杂任务为小步骤
- 危险操作前请求确认
- 使用检查点验证中间结果

### 7.2 Skills 定制

```bash
# 查看可用 Skills
openclaw skills list

# 创建自定义 Skill
openclaw skills create my-skill --template basic

# 编辑 Skill 逻辑
code ~/.openclaw/skills/my-skill/index.js
```

### 7.3 浏览器自动化

```bash
# 启动受控浏览器
openclaw browser launch

# 执行自动化任务
openclaw browser exec "打开 https://example.com 并截图"
```

> **安全警告：** 仅在可信网站上运行浏览器自动化，设置命令白名单

### 7.4 24/7 部署（VPS）

**推荐配置：**
- **最小规格：** 2 vCPU, 4GB RAM
- **存储：** 20GB SSD
- **网络：** 稳定的公网 IP

**部署步骤：**
```bash
# 使用 Docker Compose 部署
curl -O https://openclaw.ai/docker-compose.yml
docker compose up -d

# 设置开机自启
sudo systemctl enable openclaw-gateway
```

---

## 八、安全最佳实践

### 8.1 最小权限原则

```json
{
  "permissions": {
    "fileSystem": {
      "allowedPaths": ["/home/user/work"],
      "deniedPaths": ["/etc", "/root"]
    },
    "commands": {
      "whitelist": ["git", "npm", "node"],
      "blacklist": ["rm", "sudo"]
    }
  }
}
```

### 8.2 API 密钥管理

```bash
# 定期轮换密钥
openclaw secrets rotate --provider anthropic

# 检查密钥使用情况
openclaw audit api-usage --last 30d
```

### 8.3 访问控制

```json
{
  "channels": {
    "telegram": {
      "allowedUsers": ["@username1", "@username2"],
      "requirePairing": true
    }
  }
}
```

> **默认安全：** OpenClaw 默认启用 DM 配对，未知发送者需验证后才能交互

---

## 九、参考资源

### 官方文档
- **主文档站：** https://docs.openclaw.ai/
- **GitHub 仓库：** https://github.com/openclaw/openclaw
- **配置参考：** https://www.getopenclaw.ai/docs/configuration
- **故障排查：** https://docs.openclaw.ai/help/troubleshooting

### 实践教程
- **安装指南：** https://eastondev.com/blog/en/posts/ai/20260205-openclaw-installation-guide/
- **性能优化：** https://markaicode.com/reduce-openclaw-latency-5-optimization-tips/
- **成本优化：** https://eastondev.com/blog/en/posts/ai/20260205-openclaw-performance/
- **使用案例：** https://www.hostinger.com/tutorials/openclaw-use-cases

### 社区资源
- **最佳实践：** https://github.com/tobiassved/openclaw-best-practices
- **安全指南：** https://claude.ai/public/artifacts/155311d8-60e3-4143-bd90-65af86c1b721
- **多模型路由：** https://velvetshark.com/openclaw-multi-model-routing

---

## 结语

OpenClaw 的强大之处在于**持久化内存、多渠道控制和工具执行能力**的结合。通过本指南的优化技巧，你可以：

✅ 将响应延迟降低 **50%**
✅ 将运营成本降低 **60-80%**
✅ 构建 **24/7 可用**的个人 AI 助手
✅ 保持对数据的**完全控制权**

**下一步行动：**
1. 运行 `openclaw doctor` 检查当前配置
2. 启用 Prompt 缓存和智能模型路由
3. 设置一个定时任务验证自动化能力
4. 加入社区分享你的使用案例

---

*本指南基于 OpenClaw 2026 年最新版本编写，如有更新请参考官方文档。*
