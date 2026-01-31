# Clawdbot (OpenClaw) 深度调研报告

> 调研日期: 2026-02-01
> 调研模式: Deep

## TL;DR

**OpenClaw**（原名 Clawdbot）是由前 PSPDFKit 创始人 Peter Steinberger 在 2025 年创建的**开源、自托管个人 AI 助手**，定位为"Claude with hands"——不仅能聊天，更能自主执行任务（浏览器控制、文件操作、消息收发等）。项目在 GitHub 上 72 小时内突破 60,000 Stars，后超 100,000+，成为 2025-2026 年增长最快的开源项目之一。技术架构采用 Gateway-Brain-Sandbox-Skills-Nodes 五层设计，支持 15+ 消息渠道统一收件箱。**但安全问题极其严重**：Cisco、Dark Reading 等安全机构发现超过 1,100 个公开暴露的实例，存在主机入侵、凭证泄露、意外破坏性操作三大风险。2026 年 1 月因 Anthropic 商标投诉被迫更名为 OpenClaw，更名过程中还遭遇加密货币骗子抢注。**适合有安全意识的高级开发者作为个人助手使用，不推荐企业环境或安全敏感场景部署。**

---

## 核心概念

### 什么是 Clawdbot / OpenClaw？

OpenClaw 是一个**开源、自托管的个人 AI 代理**（Personal AI Agent），核心理念是让大语言模型不仅能"说"，还能"做"。与传统聊天机器人不同，OpenClaw 具备以下关键能力：

- **自主执行**：可以操作浏览器、读写文件、执行脚本、调用 API
- **多渠道整合**：将 WhatsApp、Telegram、Slack、iMessage 等 15+ 渠道统一到一个 AI 助手
- **本地优先**：所有数据存储在用户自己的机器上，不经过第三方服务器
- **模型无关**：推荐 Claude Opus 4.5，但也支持 OpenAI、本地 LLM（Llama 4、Mixtral via Ollama）

简单来说，OpenClaw 试图成为一个**真正的个人 AI 管家**——你通过任何聊天渠道给它发消息，它能理解你的意图并自主完成任务。

### 解决什么问题？

1. **碎片化的 AI 交互**：用户需要在多个聊天平台之间切换，OpenClaw 提供统一入口
2. **AI 只能"说"不能"做"**：传统聊天机器人只能对话，OpenClaw 让 AI 可以真正操控计算机
3. **隐私顾虑**：商业 AI 助手将数据存储在云端，OpenClaw 保证数据本地化
4. **可扩展性**：通过技能系统（Skills），用户可以无限扩展 AI 的能力边界

---

## 创始人背景

**Peter Steinberger**（[@steipete](https://twitter.com/steipete)），奥地利开发者，在 Apple/iOS 开发社区具有极高知名度。

- **PSPDFKit 创始人**：创建了世界领先的 PDF SDK 公司，产品覆盖 **10 亿+设备**，团队规模达 **70+ 员工**
- **倦怠与退出**：在高强度创业多年后，因严重倦怠（burnout）退出科技圈，休息了 **整整 3 年**
- **2025 年 4 月重返**：带着对 AI 的热情重新回归，创建了 Clawdbot 项目
- **开发风格**：以"build in public"著称，在社交媒体上高频分享开发进展，善于社区运营
- **技术背景**：深厚的 Apple 平台开发经验，对产品细节和用户体验有极高追求

Peter 的行业影响力和社区号召力是 OpenClaw 能在短时间内爆发增长的关键因素之一。

---

## 品牌演变历程

OpenClaw 的品牌经历了一段颇具戏剧性的演变：

### 时间线

| 时间 | 事件 |
|------|------|
| 2025 年中 | Peter Steinberger 创建 **Clawdbot** 项目，名称灵感来自 "Claude" + "bot" |
| 2025 年下半年 | 项目快速增长，GitHub Stars 72 小时内突破 60,000，社区活跃 |
| 2026 年 1 月 27 日 | **Anthropic 发出商标请求**，认为 "Clawd" 与 "Claude" 过于相似，要求更名 |
| 2026 年 1 月 27 日 | Peter 决定更名为 **Moltbot**（过渡名称），开始同时更改 GitHub 组织和 X/Twitter 账号 |
| 2026 年 1 月 27 日 | **抢注事件**：在释放旧名和注册新名的约 10 秒间隙，**加密货币骗子同时抢注了 GitHub 组织名和 X/Twitter 账号** |
| 2026 年 1 月底 | 最终定名为 **OpenClaw**，域名 openclaw.ai 上线 |
| 2026 年 2 月 | 项目稳定在 OpenClaw 品牌下运行，GitHub Stars 超过 100,000+ |

### 抢注事件始末

这是开源社区 2026 年初最戏剧性的事件之一。Peter 在执行品牌迁移时，为了保持 GitHub 组织名和社交媒体账号的一致性，选择了先释放旧名再注册新名的策略。然而，加密货币骗子（crypto squatters）显然在监控此事件，在约 **10 秒的窗口期**内同时抢注了：

- 旧的 GitHub 组织名 `clawdbot`
- 旧的 X/Twitter 账号 `@clawdbot`

这一事件引发了社区对 GitHub 组织名迁移安全机制的广泛讨论，也成为品牌迁移的反面教材。

---

## 工作原理

### 五层架构

```
┌─────────────────────────────────────────────────────┐
│                   消息渠道层                          │
│  WhatsApp │ Telegram │ Slack │ Discord │ iMessage │… │
└─────────────────┬───────────────────────────────────┘
                  │ Webhook / Polling
                  ▼
┌─────────────────────────────────────────────────────┐
│              Gateway（网关/控制平面）                   │
│  WebSocket 控制中心 ws://127.0.0.1:18789             │
│  • 消息路由与会话管理                                  │
│  • 工具调度与事件总线                                  │
│  • 多代理路由（隔离工作区 + 每代理会话）                 │
│  • 认证与权限控制                                     │
└──────┬──────────────┬──────────────┬────────────────┘
       │              │              │
       ▼              ▼              ▼
┌──────────┐  ┌──────────────┐  ┌──────────────────┐
│  Brain   │  │   Sandbox    │  │     Skills       │
│ (大脑)   │  │   (沙箱)     │  │    (技能)        │
│          │  │              │  │                  │
│ 模型无关  │  │ Docker 容器  │  │ ClawHub 注册表   │
│ 架构     │  │ 隔离执行     │  │ 可扩展模块       │
│          │  │              │  │                  │
│ 推荐:    │  │ • 文件操作   │  │ • 内置技能       │
│ Claude   │  │ • 脚本执行   │  │ • 社区技能       │
│ Opus 4.5 │  │ • 浏览器控制 │  │ • 自定义技能     │
└──────────┘  └──────────────┘  └──────────────────┘
                                        │
       ┌────────────────────────────────┘
       ▼
┌─────────────────────────────────────────────────────┐
│              Nodes（伴侣设备/节点）                    │
│  macOS │ iOS │ Android │ Headless                    │
│  通过 WebSocket 连接 Gateway                          │
│  • 相机拍照/录屏                                      │
│  • 位置获取                                           │
│  • 本地文件访问                                       │
│  • 语音唤醒                                           │
└─────────────────────────────────────────────────────┘
```

### 各层详解

#### 1. Gateway（网关）

Gateway 是整个系统的**控制平面**和中枢神经，运行在本地 `ws://127.0.0.1:18789`：

- **消息路由**：接收来自各渠道的消息，统一分发给 Brain 处理
- **会话管理**：维护多个并发会话，支持上下文隔离
- **多代理路由**：支持配置多个 AI 代理，每个代理有独立的工作区和会话
- **事件总线**：所有组件间通过事件通信，松耦合设计
- **Webhook/心跳**：支持定时任务和外部系统集成

#### 2. Brain（大脑）

Brain 是 AI 推理层，采用**模型无关架构**：

- **推荐模型**：Anthropic Claude Opus 4.5（最佳效果）
- **支持模型**：OpenAI GPT-4o/o1、本地 LLM（Llama 4、Mixtral via Ollama）
- **OAuth 订阅**：支持绑定 Anthropic Pro/Max 订阅和 OpenAI ChatGPT/Codex 订阅，降低 API 成本
- **工具调用**：Brain 决定何时调用哪些工具，实现自主代理行为

#### 3. Sandbox（沙箱）

Sandbox 提供**安全隔离的执行环境**：

- 基于 Docker 容器，与宿主机隔离
- 代码执行、文件操作、浏览器控制均在沙箱内进行
- 通过 Chrome DevTools Protocol (CDP) 控制 Chrome/Chromium 浏览器
- **注意**：非 Docker 模式下的沙箱隔离被社区质疑为不够严格

#### 4. Skills（技能）

Skills 是 OpenClaw 的**可扩展模块系统**：

- 通过 **ClawHub**（技能注册表）管理和分发
- 支持内置技能、社区技能、自定义技能
- 技能可以组合使用，形成复杂工作流
- 类似插件/扩展机制，降低二次开发门槛

#### 5. Nodes（节点）

Nodes 是**伴侣设备**，扩展 AI 对物理世界的感知：

- 支持 macOS、iOS、Android、Headless 模式
- 通过 WebSocket 连接 Gateway
- 提供相机、麦克风、位置等硬件能力
- 支持语音唤醒和对话模式

### 技术栈

| 层面 | 技术选择 |
|------|---------|
| 运行时 | Node.js ≥ 22 |
| 容器化 | Docker |
| 通信协议 | WebSocket, Webhook |
| 浏览器控制 | Chrome DevTools Protocol (CDP) |
| 语音合成 | ElevenLabs TTS |
| 可视化 | Live Canvas (A2UI) |
| 邮件集成 | Gmail Pub/Sub |

---

## 核心功能与特性

### 多渠道统一收件箱

OpenClaw 最核心的价值主张——**一个 AI 助手，所有渠道**。无论消息来自 WhatsApp、Telegram 还是 Slack，都由同一个 AI 大脑处理，并保持上下文连贯。

### 多代理路由

支持配置多个 AI 代理实例，每个代理有：
- 独立的工作区和上下文
- 专属的会话历史
- 自定义的技能配置
- 隔离的权限范围

### 语音交互

- **语音唤醒**：通过 Node 设备的麦克风实现语音唤醒
- **对话模式**：支持连续语音对话
- **TTS 输出**：集成 ElevenLabs 实现高质量语音合成

### Live Canvas

**Agent-to-UI（A2UI）** 概念的实现：
- AI 代理可以主动生成和操控可视化界面
- 支持实时数据展示、图表、交互组件
- 将 AI 的能力从纯文字扩展到视觉维度

### 浏览器控制

通过 Chrome DevTools Protocol 实现：
- 自动化网页浏览和数据采集
- 填写表单、点击按钮等交互操作
- 截图和页面内容提取
- 支持复杂的多步骤网页工作流

### 定时任务与自动化

- **Cron 定时任务**：支持定时触发 AI 执行预设工作流
- **心跳机制**：保持长连接和状态监控
- **Webhook 集成**：接收外部系统事件触发 AI 行为

### 持久记忆

- 跨会话的记忆能力
- AI 能记住用户的偏好、历史对话要点
- 为长期个人助手场景设计

### 设备能力

通过 Node 节点扩展：
- 相机拍照和录屏
- 地理位置获取
- 本地文件系统访问

---

## 支持的消息渠道

OpenClaw 支持 **15+ 消息渠道**，覆盖主流即时通讯平台：

| 渠道 | 类型 | 备注 |
|------|------|------|
| **WhatsApp** | 即时通讯 | 通过 WhatsApp Business API |
| **Telegram** | 即时通讯 | Bot API 集成 |
| **Slack** | 团队协作 | Workspace App 集成 |
| **Discord** | 社区/游戏 | Bot 集成 |
| **Google Chat** | 企业通讯 | Google Workspace 集成 |
| **Signal** | 加密通讯 | 注重隐私的渠道 |
| **iMessage** | Apple 生态 | 通过 macOS Node 实现 |
| **BlueBubbles** | iMessage 桥接 | 跨平台 iMessage 方案 |
| **Microsoft Teams** | 企业通讯 | Microsoft 365 集成 |
| **Matrix** | 开源协议 | 去中心化通讯 |
| **Zalo** | 越南主流 IM | 包含个人版和商业版 |
| **Zalo Personal** | 个人 IM | Zalo 个人账号集成 |
| **WebChat** | 网页聊天 | 内置 Web 界面 |
| **macOS** | 桌面客户端 | 原生 macOS 应用 |
| **iOS / Android** | 移动客户端 | 移动端 Node 应用 |

---

## 部署方式概览

### 1. 本地部署（推荐入门）

```bash
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

最简单的方式，适合个人开发者快速体验。需要 Node.js ≥ 22。

### 2. Docker 部署（推荐生产）

容器化部署提供更好的安全隔离，推荐用于长期运行的实例：
- Sandbox 执行环境自动容器化
- 宿主机与 AI 执行环境严格隔离
- 更容易管理和更新

### 3. 云服务器部署

支持主流云服务商：
- **AWS EC2**：适合有 AWS 基础设施的团队
- **Hetzner**：欧洲高性价比选择
- **DigitalOcean**：支持 **1-Click Deploy**，最快上手
- **Vultr**：全球多区域可选

### 4. Cloudflare Workers（实验性）

Serverless 部署方案，目前为实验性质：
- 按请求计费，低成本
- 全球边缘网络分发
- 功能可能有限制

### 5. Tailscale 远程访问

通过 Tailscale Serve/Funnel 实现：
- 安全的远程访问本地实例
- 无需公网 IP 或端口转发
- 适合在外出时访问家中运行的 OpenClaw

---

## 技能系统与生态

### ClawHub 技能注册表

ClawHub 是 OpenClaw 的技能市场/注册表，类似于 npm、Docker Hub 的概念：

- **发现技能**：浏览和搜索社区贡献的技能
- **安装技能**：一键安装到本地 OpenClaw 实例
- **发布技能**：开发者可以将自己的技能发布到 ClawHub
- **版本管理**：技能支持版本控制和更新

### 技能分类

- **内置技能**：OpenClaw 自带的基础能力（文件操作、浏览器控制等）
- **社区技能**：社区开发者贡献的扩展能力
- **自定义技能**：用户根据自己的需求开发的专属技能

### 开发模式

技能系统的设计降低了扩展门槛，开发者可以：
- 用 JavaScript/TypeScript 编写技能
- 定义技能的输入/输出规范
- 声明所需的权限和依赖
- 通过 ClawHub 分享给社区

---

## 安全风险分析

> **这是本报告最重要的章节。** OpenClaw 的安全问题是目前最大的争议焦点。

### 三大核心风险

#### 风险一：Root（主机入侵）

**严重程度：极高**

- OpenClaw 在非 Docker 模式下直接运行在宿主机上
- AI 代理拥有文件系统读写权限、脚本执行权限
- **缺乏目录沙箱**：AI 可以修改机器上的**任何文件**
- 一旦 AI 被恶意提示词（prompt injection）操纵，可能导致系统级破坏

#### 风险二：Agency（意外破坏性操作）

**严重程度：高**

- 自主代理的本质决定了它会主动执行操作
- AI 可能误解意图，执行不可逆的破坏性操作（如删除文件、发送错误消息）
- 缺乏人类确认环节（human-in-the-loop）的操作存在较高风险
- **用户反馈**：有 Hacker News 用户报告 AI 在 2 天内花费 **$300+**，说明资源消耗失控的可能性

#### 风险三：Keys（凭证泄露）

**严重程度：极高**

- OpenClaw 需要大量 API 密钥配置（Anthropic、各消息平台、ElevenLabs 等）
- 配置错误可能导致密钥泄露
- **Breached.company 调查发现**：超过 **1,100 个公开暴露的 Clawdbot 实例**，许多完全无需认证即可访问
- **HawkEye 安全研究**：发现严重配置错误导致 API 密钥、对话历史、系统完全控制权泄露

### 安全机构评估

| 来源 | 标题/观点 | 核心内容 |
|------|---------|---------|
| **Cisco 安全博客** | "Personal AI Agents like OpenClaw Are a Security Nightmare" | 个人 AI 代理的安全模型从根本上存在缺陷 |
| **Dark Reading** | OpenClaw 在企业环境中"失控" | 不适合企业环境部署，缺乏企业级安全控制 |
| **Breached.company** | 1,100+ 公开暴露实例 | 大量用户在配置时忽略安全最佳实践 |
| **HawkEye** | 严重配置错误 | API 密钥和完整对话历史可被外部访问 |

### 安全缓解措施

OpenClaw 团队提供了以下安全机制，但社区认为仍不够充分：

1. **DM Pairing（默认策略）**：将 AI 助手绑定到特定用户，防止未授权访问
2. **Docker 沙箱**：使用 Docker 容器隔离执行环境，限制 AI 对宿主机的直接访问
3. **`openclaw doctor` 检查工具**：自动检测常见配置错误和安全隐患
4. **权限分级**：部分操作需要额外确认

### 安全建议

对于有意使用 OpenClaw 的用户，建议：

1. **务必使用 Docker 部署模式**，避免直接在宿主机运行
2. **不要将实例暴露到公网**，使用 Tailscale 等工具进行安全远程访问
3. **定期运行 `openclaw doctor`** 检查配置安全性
4. **限制 API 密钥权限**，为 OpenClaw 创建专用的受限 API 密钥
5. **不要在企业或生产环境使用**，当前安全模型不适合企业场景
6. **监控 API 使用量**，设置消费上限防止成本失控

---

## 成本分析

### 软件成本

| 项目 | 费用 |
|------|------|
| OpenClaw 软件 | **免费**（MIT 开源） |
| 服务器/VPS | 取决于部署方式（本地免费，云服务器 $5-50/月） |

### API 成本（核心开销）

| 模型 | 价格 | 备注 |
|------|------|------|
| Claude Opus 4.5 API | $15/M input, $75/M output tokens | 推荐模型，效果最佳但费用最高 |
| Claude Sonnet 4 API | $3/M input, $15/M output tokens | 性价比之选 |
| OpenAI GPT-4o API | 约 $2.5-10/M tokens | 替代方案 |
| 本地 LLM (Ollama) | 免费（电费+硬件） | 效果有差距 |

### 订阅模式（降低成本）

OpenClaw 支持绑定 OAuth 订阅来降低 API 成本：

- **Anthropic Pro**：$20/月
- **Anthropic Max**：$100/月（包含更高使用量）
- **OpenAI ChatGPT Plus/Pro**：$20-200/月

### 实际使用成本参考

- **轻度使用**（每天几次简单任务）：$5-20/月
- **中度使用**（日常助手）：$50-100/月
- **重度使用**（自动化工作流、频繁调用）：**$100-300+/月**
- **Hacker News 用户报告**：2 天花费 $300+（极端案例，可能涉及循环调用或大量工具使用）

### 成本控制建议

1. 设置 API 使用量上限和告警
2. 对非关键任务使用更便宜的模型（Sonnet 而非 Opus）
3. 考虑使用 Anthropic Pro/Max 订阅而非纯 API 计费
4. 避免让 AI 执行可能产生循环调用的任务

---

## 社区与生态

### 社区规模

| 指标 | 数据 |
|------|------|
| GitHub Stars | 100,000+ |
| Discord 成员 | 8,900+ |
| GitHub 增长速度 | 72 小时内突破 60,000 Stars |

### 知名人士背书

- **Andrej Karpathy**（前 Tesla AI 总监，OpenAI 联合创始人）：公开称赞 OpenClaw
- **Chamath Palihapitiya**（Social Capital 创始人/投资人）：分享个人使用体验
- **David Sacks**（All-In Podcast 联合主持人）：推文提及
- **Casey Newton**（Platformer 创始人/科技记者）：撰写深度评测
- **MacStories**：称其为"个人 AI 助手的未来"

### 媒体关注

项目在科技媒体圈引发了广泛关注和讨论，涵盖技术评测、安全分析、使用指南等多个维度，成为 2025-2026 年最热门的开源 AI 项目之一。

---

## 适用场景 / 不适用场景

### 适用场景

| 场景 | 说明 |
|------|------|
| **个人 AI 助手** | 技术用户的日常助手，管理消息、执行任务 |
| **自动化工作流** | 定时任务、数据采集、报告生成等重复性工作 |
| **多平台消息管理** | 统一管理多个聊天平台的消息和回复 |
| **原型验证** | 快速验证 AI 代理能力的概念原型 |
| **学习和研究** | 了解自主 AI 代理的架构设计和实现 |
| **隐私敏感场景** | 需要数据完全本地化的用户 |

### 不适用场景

| 场景 | 原因 |
|------|------|
| **企业生产环境** | 安全模型不够成熟，缺乏企业级审计、权限控制 |
| **面向客户的服务** | 稳定性和安全性不足以支撑客户交互 |
| **处理敏感数据** | 尽管数据本地化，但 AI 代理的行为不可完全预测 |
| **预算有限的用户** | API 成本可能快速增长，不适合预算紧张的场景 |
| **非技术用户** | 安装配置需要一定技术能力，安全配置更是如此 |
| **团队协作** | 当前设计以个人使用为主，缺乏团队协作功能 |

---

## 与同类产品对比

| 项目 | 定位 | 优势 | 劣势 | 适用场景 | GitHub Stars | 最近更新 |
|------|------|------|------|---------|-------------|---------|
| **OpenClaw** | 开源自托管个人 AI 代理 | 功能最全面、15+ 渠道、自托管隐私优先、社区庞大 | 安全风险高、配置复杂、API 成本高 | 技术用户个人助手 | 100K+ | 2026-01 |
| **Open Interpreter** | 本地代码执行 AI 助手 | 简单易用、代码执行能力强 | 仅限代码执行，无多渠道支持 | 开发者编程助手 | ~55K | 持续更新 |
| **AutoGPT** | 通用自主 AI 代理 | 先发优势、概念验证完善 | 实用性有限、容易陷入循环 | AI 代理实验研究 | ~170K | 持续更新 |
| **CrewAI** | 多代理协作框架 | 多代理编排能力强、Python 生态 | 偏框架非成品、需开发 | 团队 AI 工作流 | ~25K | 持续更新 |
| **n8n + AI** | 工作流自动化 + AI | 可视化工作流、企业级稳定性 | AI 能力是附加非核心 | 企业自动化工作流 | ~55K | 持续更新 |
| **Home Assistant + AI** | 智能家居 + AI 控制 | IoT 集成能力强、社区成熟 | 专注智能家居领域 | 智能家居 AI 控制 | ~75K | 持续更新 |

### 关键差异化

OpenClaw 的独特定位在于：
1. **渠道覆盖最广**：15+ 消息平台，这是竞品难以匹敌的
2. **"Personal AI"定位清晰**：不是框架/工具库，而是开箱即用的个人助手
3. **Node 设备概念**：将手机/电脑变成 AI 的"手脚"，扩展物理世界感知

---

## 最新动态 (2025-2026)

| 时间 | 事件 |
|------|------|
| 2025 年 4 月 | Peter Steinberger 重返科技圈，开始 Clawdbot 项目 |
| 2025 年中 | 项目快速增长，获得大量关注 |
| 2025 年下半年 | Andrej Karpathy、Chamath 等知名人士公开背书 |
| 2025 年末 | Cisco、Dark Reading 等发布安全警告 |
| 2025 年末 | Breached.company 发现 1,100+ 暴露实例 |
| 2026 年 1 月 27 日 | Anthropic 商标请求，被迫更名 |
| 2026 年 1 月 27 日 | 更名过程中遭加密货币骗子抢注旧账号 |
| 2026 年 1 月底 | 最终定名 OpenClaw，openclaw.ai 上线 |
| 2026 年 2 月 | GitHub Stars 超过 100,000+，项目在新品牌下持续迭代 |

---

## 参考来源

| ID | 来源 | 类型 | URL |
|---|---|---|---|
| S1 | GitHub 项目主页 | Tier A | https://github.com/openclaw/openclaw |
| S2 | OpenClaw 官网 | Tier A | https://openclaw.ai |
| S3 | OpenClaw 官方文档 | Tier A | https://docs.openclaw.ai |
| S4 | Clawd.bot（原域名） | Tier A | https://clawd.bot |
| S5 | Cisco 安全博客 | Tier A | https://blogs.cisco.com/ai/personal-ai-agents-like-openclaw-are-a-security-nightmare |
| S6 | Dark Reading 安全分析 | Tier A | https://www.darkreading.com/application-security/openclaw-ai-runs-wild-business-environments |
| S7 | Breached.company 暴露实例报告 | Tier B | https://breached.company/over-1-000-clawdbot-ai-agents-exposed-on-the-public-internet-a-security-wake-up-call-for-autonomous-ai-infrastructure/ |
| S8 | HawkEye 安全研究 | Tier B | https://hawk-eye.io/2026/01/the-clawdbot-vulnerability-how-a-hyped-ai-agent-became-a-security-liability/ |
| S9 | Mashable 安全风险分析 | Tier B | https://mashable.com/article/clawdbot-ai-security-risks |
| S10 | CNET 品牌重命名报道 | Tier B | https://www.cnet.com/tech/services-and-software/from-clawdbot-to-moltbot-to-openclaw/ |
| S11 | PCMag 安全评估 | Tier B | https://www.pcmag.com/news/clawdbot-now-moltbot-is-hot-new-ai-agent-safe-to-use-or-risky |
| S12 | DEV Community 技术介绍 | Tier B | https://dev.to/sivarampg/clawdbot-the-ai-assistant-thats-breaking-the-internet-1a47 |
| S13 | DEV Community 重命名事件 | Tier B | https://dev.to/sivarampg/from-clawdbot-to-moltbot-how-a-cd-crypto-scammers-and-10-seconds-of-chaos-took-down-the-4eck |
| S14 | MacStories 评测 | Tier B | https://www.macstories.net/stories/clawdbot-showed-me-what-the-future-of-personal-ai-assistants-looks-like/ |
| S15 | Platformer / Casey Newton 评测 | Tier B | https://www.platformer.news/moltbot-clawdbot-review-ai-agent/ |
| S16 | Hacker News 社区讨论 | Tier C | https://news.ycombinator.com/item?id=46760237 |
| S17 | Pragmatic Engineer 创始人专访 | Tier A | https://newsletter.pragmaticengineer.com/p/the-creator-of-clawd-i-ship-code |
| S18 | 36Kr 创始人深度访谈 | Tier B | https://eu.36kr.com/en/p/3660257828594306 |
| S19 | Fast Company 成本分析 | Tier B | https://www.fastcompany.com/91484506/what-is-clawdbot-moltbot-openclaw |
| S20 | DigitalOcean 部署指南 | Tier A | https://www.digitalocean.com/resources/articles/what-is-openclaw |
| S21 | Vultr 部署文档 | Tier A | https://docs.vultr.com/how-to-deploy-openclaw-autonomous-ai-agent-platform |
| S22 | Pulumi 云部署博客 | Tier B | https://www.pulumi.com/blog/deploy-openclaw-aws-hetzner/ |
| S23 | Cloudflare Moltworker | Tier A | https://github.com/cloudflare/moltworker |
| S24 | Awesome OpenClaw Skills | Tier B | https://github.com/VoltAgent/awesome-openclaw-skills |
| S25 | ClawHub 技能注册表 | Tier A | https://github.com/openclaw/clawhub |
| S26 | Composio 安全加固指南 | Tier B | https://composio.dev/blog/secure-moltbot-clawdbot-setup-composio |
| S27 | Mashable 产品介绍 | Tier B | https://mashable.com/article/what-is-clawdbot-how-to-try |
| S28 | LinkedIn 对比评测 | Tier C | https://www.linkedin.com/posts/nateherkelman_i-tested-clawdbot-against-claude-code-what-activity-7422128224358330369-PfLW |

---

Research Metadata:
- Mode: Deep
- Sources consulted: 28
- Search passes: 3
- Tools used: WebSearch, mcp__exa__web_search_exa, mcp__open-websearch__search, mcp__mcp-deepwiki__deepwiki_fetch, mcp__open-websearch__fetchGithubReadme, WebFetch
- Evidence confidence: High（多源交叉验证）
- Date: 2026-02-01

---
