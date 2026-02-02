# Clawdbot (Moltbot) 深度调研报告

> 日期：2026-01-31

## TL;DR

Clawdbot（现已更名为 Moltbot）是由 PSPDFKit 创始人 Peter Steinberger 开发的**开源自托管个人 AI 助手**。它将 Claude AI 接入 WhatsApp、Telegram、Discord 等日常通讯应用，不仅能"对话"，还能在你的电脑上**执行真实操作**（运行命令、处理文件、发邮件、控制浏览器等）。2026 年 1 月爆火，24 小时内获得 9,000 GitHub Stars，一周内突破 **100,000 Stars**，成为 GitHub 历史上增长最快的开源项目之一。因 Anthropic 商标请求被迫更名为 Moltbot。虽然功能强大，但安全隐患不容忽视——大量企业实例暴露在公网且缺乏认证，存在凭据泄露、供应链攻击等风险。

---

## 核心概念

### 什么是 Clawdbot/Moltbot？

Clawdbot 是一个**本地运行的 AI Agent 平台**，核心理念是：

| 特性 | 说明 |
|------|------|
| 🤖 **个人 AI 助手** | 通过你日常使用的聊天应用（WhatsApp、Telegram 等）与 AI 对话 |
| 💻 **真正的执行力** | 不只是聊天——能执行终端命令、处理文件、发邮件、控制浏览器 |
| 🔒 **完全本地运行** | 数据不离开你的设备，完全掌控隐私 |
| 🔗 **多平台统一** | 一个 AI 助手，多个通讯平台同步接入 |
| 🧠 **持久记忆** | 跨平台的持久化记忆系统，记住你的偏好和上下文 |
| ⏰ **主动推送** | 支持定时任务、提醒推送，不只是被动回答 |

### 与传统 AI 聊天的区别

| 对比维度 | Clawdbot/Moltbot | 传统 AI Chat（ChatGPT/Claude Web） |
|----------|------------------|------------------------------------|
| **使用方式** | 在日常聊天应用中直接使用 | 需要打开专门的网站 |
| **对话记忆** | 跨平台持久记忆 | 会话通常独立 |
| **主动消息** | 支持定时提醒和通知 | 仅被动响应 |
| **数据存储** | 本地 Markdown 文件 | 存储在云端 |
| **系统操作** | 可执行终端命令、文件操作 | 无法操作本地系统 |
| **自定义能力** | 完全可定制（Skills 系统） | 有限定制 |
| **运行时间** | 24/7 持续运行 | 仅在打开网页时 |

---

## 工作原理（架构解析）

### 系统架构概览

```
👤 用户
  │
  ▼
📱 聊天平台（WhatsApp / Telegram / Discord / Slack / iMessage / Signal / Teams ...）
  │
  ▼
🌐 WebSocket Gateway（消息路由中心）
  │
  ▼
🧠 Claude AI Agent（智能引擎）
  │         │
  ▼         ▼
🛠️ Tool    📁 Memory
  System     System
  │
  ▼
📤 回复投递（多通道广播）
  │
  ▼
👤 用户收到回复
```

### 核心组件

#### 1. Gateway（网关层）

- 基于 **WebSocket** 协议的消息路由中心
- 负责连接各聊天平台与 AI Agent
- 支持 HTTP/SSE 降级方案（受限网络环境）
- 支持多网关部署（Multi-Gateway）

#### 2. Agent（智能代理层）

- 基于 Claude API 的 Agent Loop 系统
- 支持 **System Prompt** 自定义
- **Context 管理** + **Token 使用跟踪**
- **Session 管理**：会话创建、修剪（pruning）、压缩（compaction）
- **Multi-Agent Routing**：多代理路由，不同渠道/用户可路由到不同 Agent

#### 3. Memory（记忆系统）

- 基于本地 **Markdown 文件**存储
- 持久化用户偏好、对话历史、项目信息
- Agent Workspace：每个 Agent 有独立的工作空间

#### 4. Tool System（工具系统）

- 内置工具：终端命令、文件操作、浏览器控制、邮件、日历等
- **Skills 系统**：社区贡献的可复用技能包（类似插件）
- **Plugins 系统**：更深层的扩展能力
- **Hooks 系统**：事件钩子，支持自定义触发逻辑

#### 5. 支持的通讯平台（16+）

| 平台 | 状态 |
|------|------|
| 💬 WhatsApp | ✅ 支持 |
| ✈️ Telegram | ✅ 支持 |
| 🎮 Discord | ✅ 支持 |
| 💼 Slack | ✅ 支持 |
| 📱 iMessage | ✅ 支持 |
| 🔐 Signal | ✅ 支持 |
| 🏢 Microsoft Teams | ✅ 支持 |
| 🌐 WebChat | ✅ 支持 |
| 💬 Google Chat | ✅ 支持 |
| 📱 BlueBubbles | ✅ 支持 |
| 🔒 Matrix | ✅ 支持 |
| 💬 Zalo | ✅ 支持 |
| 🖥️ macOS 原生 | ✅ 支持 |
| 📱 iOS/Android | ✅ 支持 |

### 技术栈

- **运行时**：支持 Node.js / Bun
- **AI 模型**：Claude (Anthropic) 为主，也支持 OpenAI 模型
- **模型故障转移**：Model Failover 机制，自动切换备用模型
- **协议**：WebSocket + 自定义 Bridge Protocol
- **部署方式**：本地安装 / Docker / Railway / Render / Northflank / Nix / Ansible

---

## 发展历程与里程碑

| 时间 | 事件 |
|------|------|
| 2025 年底 | Peter Steinberger 作为"业余项目"开发 Clawdbot |
| 2026-01-24 | 项目爆火，24 小时内获 9,000 Stars |
| 2026-01-24~27 | 3 天内飙升至 54,000 Stars → 82,000 Stars |
| 2026-01-27 | Anthropic 发出商标请求，被迫更名为 **Moltbot**（龙虾蜕壳之意） |
| 2026-01-27 | GitHub 页面一度被**加密货币诈骗者**接管 |
| 2026-01-27 | 更名引发**仿冒攻击活动**（Malwarebytes 报告） |
| 2026-01-28 | GitHub Stars 突破 **100,000**，Discord 社区超 8,900 人 |
| 2026-01-28 | 多家安全公司发布安全警告（Noma、BitDefender、SOC Prime、BleepingComputer） |
| 2026-01-28 | Cloudflare 发布 **Moltworker**（基于 Cloudflare Workers 的部署方案） |

---

## 与竞品对比

### Clawdbot/Moltbot vs Claude Code vs Cursor

| 对比维度 | Clawdbot/Moltbot | Claude Code | Cursor |
|----------|------------------|-------------|--------|
| **定位** | 全能个人 AI 助手 | 终端原生 AI 编程工具 | AI-First IDE |
| **交互方式** | 通讯应用（WhatsApp 等） | 终端 CLI | VS Code Fork GUI |
| **核心能力** | 生活+工作全场景 Agent | 深度代码理解与修改 | 实时代码补全与编辑 |
| **运行方式** | 本地 24/7 后台运行 | 按需启动 | IDE 内持续运行 |
| **编程能力** | ✅ 有（但非专长） | ✅ 极强（专业级） | ✅ 极强（专业级） |
| **非编程任务** | ✅ 邮件/日历/提醒/研究 | ❌ 专注编程 | ❌ 专注编程 |
| **多平台通讯** | ✅ 16+ 平台 | ❌ 无 | ❌ 无 |
| **持久记忆** | ✅ 本地文件 | ⚠️ 有限（CLAUDE.md） | ⚠️ 有限（.cursorrules） |
| **开箱即用** | ⭐ 9/10 | ⭐ 7/10 | ⭐ 8/10 |
| **设置难度** | 中等（30-60 分钟） | 低（几分钟） | 低（几分钟） |
| **安全风险** | ⚠️ 高（需 root 权限） | ⚠️ 中（沙箱保护） | ⚠️ 低（IDE 隔离） |
| **成本** | 免费 + API 费用 | $20-100/月 订阅 | $20-200/月 订阅 |
| **开源** | ✅ 完全开源 | ❌ 闭源 | ❌ 闭源 |
| **社区** | 100K+ Stars，活跃 | Anthropic 官方支持 | 大型用户社区 |

### Nate Herkelman 的评测评分（100 小时测试）

| 指标 | Clawdbot | Claude Code | 说明 |
|------|----------|-------------|------|
| 开箱即用能力 | 9 | 7 | Clawdbot 功能更全面 |
| 设置摩擦与风险 | 6 | 8 | Claude Code 设置更简单安全 |
| 成本 | 7 | 6 | Clawdbot 免费但有 API 费用 |
| 功能与权限 | 9 | 7 | Clawdbot 系统级访问更深 |
| 安全性 | 5 | 8 | Clawdbot 安全风险显著更高 |
| 日常可用性 | 8 | 7 | 通讯应用集成更便捷 |
| 实际 ROI | 7 | 8 | Claude Code 编程场景 ROI 更高 |
| **综合评价** | Claude Code 仍然胜出，但 Clawdbot 在非编程场景有独特优势 |

---

## 安全风险深度分析 ⚠️

朋友们，这个部分**非常非常关键**！安全问题是 Clawdbot 目前最大的隐患。

### 🚨 已发现的安全问题

#### 1. 暴露的管理面板（Critical）

- 安全研究人员发现**数百个 Clawdbot Control 管理面板**暴露在公网
- 反向代理配置错误导致未经认证即可访问
- 可查看：API 密钥、OAuth Token、完整对话历史、文件交换记录
- **来源**：[BleepingComputer](https://www.bleepingcomputer.com/news/security/viral-moltbot-ai-assistant-raises-concerns-over-data-security/)、[Bitdefender](https://www.bitdefender.com/en-us/blog/hotforsecurity/moltbot-security-alert-exposed-clawdbot-control-panels-risk-credential-leaks-and-account-takeovers)

#### 2. 明文存储凭据（High）

- 用户凭据以**明文 Markdown 和 JSON 文件**存储
- 容易被 RedLine、Lumma、Vidar 等信息窃取木马获取
- **来源**：[SOC Prime](https://socprime.com/active-threats/the-moltbot-clawdbots-epidemic/)

#### 3. 供应链攻击风险（High）

- Skills 库可被投毒——研究人员演示了上传恶意 Skill 到 ClawdHub 实现远程命令执行
- **来源**：[SOC Prime](https://socprime.com/active-threats/the-moltbot-clawdbots-epidemic/)

#### 4. 企业内部无序扩散（High）

- Noma Security 报告：**53% 的企业客户在一个周末内给了 ClawdBot 特权访问**，且安全团队毫不知情
- 影子 IT 问题严重
- **来源**：[Noma Security](https://noma.security/blog/customers-gave-clawdbot-privileged-access-and-noone-asked-permission/)

#### 5. 更名引发仿冒活动（Medium）

- 从 Clawdbot 更名为 Moltbot 的过程中，攻击者利用混乱发起仿冒攻击
- GitHub 页面一度被加密货币诈骗者接管
- **来源**：[Malwarebytes](https://www.malwarebytes.com/blog/threat-intel/2026/01/clawdbots-rename-to-moltbot-sparks-impersonation-campaign)

### 🛡️ 安全建议

| 建议 | 说明 |
|------|------|
| ✅ 强制认证 | 所有 Moltbot 服务必须启用强认证 |
| ✅ 关闭管理端口 | 防火墙封锁管理端口，禁止公网访问 |
| ✅ 加密存储 | 启用静态加密存储敏感凭据 |
| ✅ 沙箱隔离 | 容器化运行，限制系统权限 |
| ✅ 审计 Skills | 谨慎安装第三方 Skills，审查来源 |
| ✅ 最小权限 | 不要赋予 root 权限，按需授权 |
| ⚠️ 企业管控 | 安全团队应监控 Moltbot 在内部的部署情况 |

---

## 最新动态（2026 年 1 月）

1. **🦞 更名为 Moltbot**（2026-01-27）：因 Anthropic 商标请求更名，"Molt"取龙虾蜕壳之意。网站从 `clawd.bot` 迁移至 `molt.bot`
2. **⭐ 突破 100,000 GitHub Stars**（2026-01-28）：成为 GitHub 历史上增长最快的项目之一
3. **☁️ Cloudflare Moltworker**（2026-01-28）：Cloudflare 发布基于 Workers 的自托管方案，无需 Mac Mini
4. **🚨 安全警报频发**（2026-01-27~28）：多家安全公司（Noma、Bitdefender、SOC Prime、BleepingComputer）发布安全警告
5. **🎯 Mac Mini 销量暴增**：因用户寻找专用运行设备，间接推动了 Mac Mini 的销售
6. **👏 业界认可**：AI 研究者 Andrej Karpathy 公开称赞；MacStories 称其为"个人 AI 助手的未来"
7. **💰 投资人关注**：Chamath Palihapitiya 分享 Moltbot 帮他省了 15% 车险费用

---

## 适用场景 / 不适用场景

### ✅ 适合使用的场景

| 场景 | 说明 |
|------|------|
| 🏠 **个人生活助手** | 日程管理、邮件处理、提醒推送、信息检索 |
| 💬 **多平台统一通讯** | 一个 AI 助手跨 WhatsApp/Telegram/Discord 等 |
| 🔧 **开发者自动化** | 代码提交、PR 管理、构建监控、日报生成 |
| 📊 **数据处理** | 文件处理、报告生成、数据分析 |
| 🏡 **智能家居控制** | 配合 Home Assistant 等平台控制设备 |
| 🧪 **技术爱好者探索** | 了解 AI Agent 架构、学习 Agent 开发 |

### ❌ 不适合使用的场景

| 场景 | 原因 |
|------|------|
| 🏢 **企业生产环境** | 安全机制不够成熟，凭据管理存在隐患 |
| 🔒 **高安全要求场景** | 明文存储、缺乏审计日志、供应链风险 |
| 👶 **零技术基础用户** | 需要终端操作能力，安全配置需要专业知识 |
| 💻 **纯编程场景** | Claude Code / Cursor 在编程领域更专业高效 |
| 🏥 **敏感数据处理** | 医疗/金融等敏感数据不建议交给目前版本处理 |

---

## 快速入门（Getting Started）

### 前置条件

- macOS / Linux / Windows (WSL)
- 终端操作基础
- Anthropic API Key 或 OpenAI API Key

### 安装方式

#### 方式一：一行命令安装（推荐）

```bash
# 官方安装脚本
curl -fsSL https://get.molt.bot | bash
```

#### 方式二：Docker 部署

```bash
docker run -d --name moltbot \
  -e ANTHROPIC_API_KEY=your_key \
  -v moltbot_data:/data \
  ghcr.io/openclaw/openclaw:latest
```

#### 方式三：Bun 运行

```bash
bun install openclaw
bunx openclaw setup
```

### 配置流程

1. **运行安装脚本** → 自动下载并配置
2. **配置 AI 模型** → 输入 Anthropic/OpenAI API Key
3. **连接通讯平台** → 扫码绑定 WhatsApp / 配置 Telegram Bot Token 等
4. **开始对话** → 在你的聊天应用中直接与 AI 对话

### 安全配置（强烈建议）

```bash
# 使用 CLI 进行安全检查
openclaw doctor

# 配置认证
openclaw security --enable-auth

# 限制网络访问
openclaw configure --bind localhost
```

---

## 创始人背景

**Peter Steinberger**

- PSPDFKit（现 Nutrient）创始人，知名 iOS/PDF 技术公司
- Apple 开发者社区知名人物
- 将 Clawdbot 定位为"业余项目"（hobby project）
- 项目爆火后表示会持续投入开发

---

## 总结与展望

### 优势 💪

- **革命性的交互方式**：将 AI 融入日常通讯，降低使用门槛
- **完全开源免费**：代码透明，社区驱动
- **本地运行保护隐私**：数据不上云
- **强大的执行能力**：不只是聊天，能真正做事
- **活跃的社区生态**：565+ 社区 Skills，8,900+ Discord 成员

### 劣势 ⚠️

- **安全性严重不足**：明文凭据、暴露端口、供应链风险
- **项目极度年轻**：2025 年底才开始，稳定性待验证
- **设置复杂度较高**：对非技术用户不友好
- **命名/品牌混乱**：经历 Clawdbot → Moltbot 更名，社区认知混乱
- **企业就绪度低**：缺乏审计、合规、权限管理等企业级特性

### 前景判断

Clawdbot/Moltbot 代表了**个人 AI Agent 的重要方向**——将 AI 从网页聊天窗口解放出来，融入用户的真实工作和生活场景。100K Stars 的爆发式增长证明了市场需求的真实存在。但目前它更像是一个**精彩的技术原型**，距离生产级安全可靠还有相当长的路要走。

**建议：可以作为个人学习和探索使用，暂不建议在企业环境或敏感场景部署。**

---

## 参考来源

- [GitHub - openclaw/openclaw](https://github.com/clawdbot/clawdbot) - 官方 GitHub 仓库
- [Moltbot 官方文档](https://docs.molt.bot/) - 架构与使用文档
- [How Moltbot Works](https://moltbot.you/how-moltbot-works.html) - 技术深度分析
- [Mashable: Clawdbot is now Moltbot](https://mashable.com/article/clawdbot-changes-name-to-moltbot) - 更名报道
- [36Kr: Clawdbot Hits 100,000+ Stars](https://eu.36kr.com/en/p/3661357114942337) - Stars 突破 10 万
- [BleepingComputer: Viral Moltbot raises concerns](https://www.bleepingcomputer.com/news/security/viral-moltbot-ai-assistant-raises-concerns-over-data-security/) - 安全风险报道
- [Noma Security: 53% Enterprise Exposure](https://noma.security/blog/customers-gave-clawdbot-privileged-access-and-noone-asked-permission/) - 企业安全风险
- [Bitdefender: Exposed Control Panels](https://www.bitdefender.com/en-us/blog/hotforsecurity/moltbot-security-alert-exposed-clawdbot-control-panels-risk-credential-leaks-and-account-takeovers) - 暴露管理面板
- [SOC Prime: Moltbot Epidemic](https://socprime.com/active-threats/the-moltbot-clawdbots-epidemic/) - 供应链攻击风险
- [Malwarebytes: Impersonation Campaign](https://www.malwarebytes.com/blog/threat-intel/2026/01/clawdbots-rename-to-moltbot-sparks-impersonation-campaign) - 仿冒攻击活动
- [Cloudflare: Moltworker](https://blog.cloudflare.com/moltworker-self-hosted-ai-agent/) - Cloudflare Workers 部署方案
- [DataCamp: Moltbot Tutorial](https://www.datacamp.com/tutorial/moltbot-clawdbot-tutorial) - 入门教程
- [Analytics Vidhya: Clawdbot Guide](https://www.analyticsvidhya.com/blog/2026/01/clawdbot-guide/) - 实测指南
- [Dev.to: From Clawdbot to Moltbot](https://dev.to/sivarampg/from-clawdbot-to-moltbot-how-a-cd-crypto-scammers-and-10-seconds-of-chaos-took-down-the-4eck) - 更名风波全记录
- [Nate Herkelman: 100 Hours Testing](https://www.youtube.com/watch?v=CBNbcbMs_Lc) - Clawdbot vs Claude Code 100 小时对比评测
- [Medium: Production Guide](https://alirezarezvani.medium.com/everyones-installing-moltbot-clawdbot-here-s-why-i-m-not-running-it-in-production-yet-04f9ec596ef5) - 生产环境部署评估
