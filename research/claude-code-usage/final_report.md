# Claude Code 使用方式与高效技巧：从入门到精通

> **版本**: V1.0 (Final) | **日期**: 2026-02-10 | **字数**: 约15,000字
> **适用版本**: Claude Code 最新稳定版 | **语言**: 简体中文

---

## 目录

- [第一部分：快速入门](#第一部分快速入门)
  - [1. Claude Code 简介](#1-claude-code-简介)
  - [2. 安装与配置](#2-安装与配置)
- [第二部分：核心功能详解](#第二部分核心功能详解)
  - [3. 基础使用](#3-基础使用)
  - [4. 工作模式](#4-工作模式)
  - [5. 上下文管理](#5-上下文管理)
- [第三部分：高级特性](#第三部分高级特性)
  - [6. Skills 技能系统](#6-skills-技能系统)
  - [7. MCP Server 集成](#7-mcp-server-集成)
  - [8. Hooks 钩子系统](#8-hooks-钩子系统)
  - [9. Subagents 子代理](#9-subagents-子代理)
- [第四部分：工作流优化](#第四部分工作流优化)
  - [10. 高效工作流](#10-高效工作流)
  - [11. 效率提升技巧](#11-效率提升技巧)
  - [12. 性能优化](#12-性能优化)
- [第五部分：问题解决](#第五部分问题解决)
  - [13. 常见问题与解决方案](#13-常见问题与解决方案)
  - [14. 调试技巧](#14-调试技巧)
- [附录](#附录)

---

# 第一部分：快速入门

## 1. Claude Code 简介

### 1.1 什么是 Claude Code

Claude Code 是 Anthropic 公司推出的一款 **AI 驱动的命令行编程助手**，它将 Claude 大语言模型的能力直接带入终端环境 [S002]。与传统的 IDE 插件或 Web 聊天界面不同，Claude Code 定位为一个 **系统级 AI Agent**——它不仅能理解和生成代码，还能直接操作文件系统、执行终端命令、管理 Git 仓库，并与外部工具和服务进行集成 [S026]。

从架构设计理念来看，Claude Code 采用了 **"终端即操作系统"** 的设计哲学。它将 AI 的推理能力与操作系统的执行能力融合，使得 AI 不再局限于"建议"代码，而是能够"执行"完整的开发工作流 [S026]。简而言之，Claude Code 就像一位驻扎在你终端里的高级工程师，能够阅读你的整个代码库，理解项目上下文，并执行从代码编写到测试、部署的完整开发流程 [S009]。

### 1.2 核心特性与优势

Claude Code 的核心竞争力体现在以下四个维度：

| 特性维度 | 具体能力 | 架构意义 |
|---------|---------|---------|
| **超大上下文窗口** | 200K tokens [S002, S026] | 可一次性理解大型代码库的完整上下文，无需频繁分段处理 |
| **系统级权限访问** | 文件读写、Shell 执行、Git 操作 [S026] | 从"建议者"升级为"执行者"，实现端到端的任务完成 |
| **三重扩展体系** | Skills / MCP / Hooks [S008, S026] | 分层解耦的扩展架构：知识层(Skills)、工具层(MCP)、流程层(Hooks) |
| **多代理协作能力** | Subagents / 多终端 [S020, S026] | 支持任务并行化与职责分离，适合复杂工程项目 |

**与传统 AI 编程工具对比：**

| 对比维度 | 传统 AI 插件(如 Copilot) | Claude Code |
|---------|------------------------|-------------|
| 运行环境 | IDE 插件 | 独立终端 Agent |
| 操作权限 | 代码补全/建议 | 系统级读写执行 |
| 上下文范围 | 当前文件/相邻文件 | 200K tokens 全项目 |
| 扩展能力 | 有限 | Skills + MCP + Hooks |
| 自动化程度 | 辅助编写 | 端到端任务完成 |

### 1.3 适用场景

Claude Code 最擅长的开发场景包括 [S003, S019]：

| 场景 | 说明 | 典型命令 |
|------|------|----------|
| **代码理解与问答** | 快速理解陌生代码库的架构和逻辑，特别适合接手遗留项目 | `解释这个项目的目录结构` |
| **大规模重构** | 跨文件的代码重组、API 迁移、框架升级 | `将所有 class 组件迁移到 hooks` |
| **调试与错误修复** | 结合执行能力，自动运行测试、定位问题、生成修复方案 [S036] | `分析这个错误日志并修复` |
| **复杂功能生成** | 生成完整的功能模块（含测试） | `创建一个用户认证模块` |
| **测试驱动开发** | 编写测试用例并实现通过代码 [S003, S012] | `为 UserService 编写单元测试` |
| **Git 操作自动化** | 自动生成 commit 信息、创建 PR | `/commit` |
| **CI/CD 管道集成** | 以 headless 模式嵌入自动化流水线 [S012] | `claude -p "审查 PR"` |

---

## 2. 安装与配置

### 2.1 系统要求

在安装 Claude Code 之前，请确保系统满足以下要求 [S001, S033]：

| 要求 | 最低版本 | 推荐版本 | 备注 |
|------|----------|----------|------|
| **Node.js** | 18.0+ | 20 LTS | 核心运行时依赖 |
| **操作系统** | macOS 12+ / Ubuntu 20.04+ / Windows (WSL2) | macOS 14+ / Ubuntu 22.04+ | Windows 需通过 WSL [S005] |
| **内存** | 4 GB | 8 GB+ | MCP Server 较多时需更多内存 |
| **磁盘空间** | 500MB | 1GB+ | 包含依赖和缓存 |
| **网络** | 稳定连接 | 低延迟连接 | 需访问 Anthropic API |

检查 Node.js 版本：

```bash
node --version
# 确保输出 >= v18.0.0
```

如果版本不够，使用 nvm 升级：

```bash
# 安装 nvm（如果尚未安装）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# 安装并使用 Node.js 20 LTS
nvm install 20
nvm use 20
```

### 2.2 安装方法

Claude Code 提供三种安装方式，根据你的环境和偏好选择 [S001, S009]：

#### 方法一：原生安装（推荐）

这是官方推荐的安装方式，适用于 macOS 和 Linux [S001]：

```bash
# macOS / Linux 一键安装
curl -fsSL https://claude.ai/install.sh | bash
```

该脚本会自动检测系统环境，安装 Claude Code 到 `~/.local/bin/` 目录。

#### 方法二：Homebrew 安装（macOS）

适合已经使用 Homebrew 管理工具的 macOS 用户 [S001]：

```bash
# 通过 Homebrew 安装
brew install claude-code
```

#### 方法三：npm 全局安装

适合所有平台，尤其是需要精确控制版本的场景 [S001, S009]：

```bash
# npm 全局安装
npm install -g @anthropic-ai/claude-code

# 或使用 yarn
yarn global add @anthropic-ai/claude-code
```

#### 安装方式选择决策

| 场景 | 推荐方式 | 理由 |
|------|---------|------|
| macOS 首次安装 | 原生安装 | 自动配置，零门槛 |
| macOS Homebrew 用户 | Homebrew | 统一包管理 |
| Linux 服务器 | npm | 依赖 Node.js 环境通常已有 |
| CI/CD 环境 | npm | 可锁定版本，可重复构建 |
| Windows | WSL2 + 原生安装 | 必须通过 WSL2 [S005] |

#### Windows 特别说明

Claude Code 不原生支持 Windows 命令行。Windows 用户需要通过 **WSL2 (Windows Subsystem for Linux)** 使用 [S005, S033]：

```bash
# 步骤 1：安装 WSL2
wsl --install

# 步骤 2：在 WSL2 中安装 Node.js
# 注意：必须使用 Linux 版本的 Node.js，不要使用 Windows 版本
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
nvm install 20

# 步骤 3：在 WSL2 中安装 Claude Code
npm install -g @anthropic-ai/claude-code
```

> **重要提示**：在 WSL 环境中，确保使用的是 Linux 原生的 Node.js，而不是 Windows 安装的版本。运行 `which node` 确认路径指向 `/home/` 或 `/usr/` 开头的路径 [S005, S033]。

### 2.3 首次配置

安装完成后，需要进行首次配置 [S001, S032]：

```bash
# 步骤 1：启动 Claude Code
claude

# 步骤 2：系统会提示登录认证
# 方式 A：通过浏览器登录 Anthropic 账户（自动打开浏览器）
# 方式 B：使用 API Key

# 如果使用 API Key，设置环境变量
export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxx"

# 建议将其添加到 shell 配置文件中
echo 'export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxx"' >> ~/.zshrc
# 或
echo 'export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxx"' >> ~/.bashrc
```

认证方式有两种 [S001, S032]：

1. **Anthropic 账户登录**：通过浏览器 OAuth 认证，适合个人使用
2. **API Key 认证**：设置 `ANTHROPIC_API_KEY` 环境变量，适合自动化场景和 CI/CD

> **提示**：如果浏览器未自动打开，可手动复制终端输出的 URL 在浏览器中完成登录 [S032]。

### 2.4 验证安装

完成安装和配置后，运行以下命令验证 [S001]：

```bash
# 检查版本
claude --version

# 启动交互会话并测试
claude

# 在会话中输入简单问题验证
> 你好，请介绍一下自己
```

如果看到 Claude 的回复，说明安装和配置均已成功。

常见安装验证问题的快速排查 [S032, S033]：

```bash
# 如果提示 "command not found"
# 检查 PATH 是否包含安装目录
echo $PATH | tr ':' '\n' | grep -E "local/bin|node_modules"

# 手动添加到 PATH
export PATH="$HOME/.local/bin:$PATH"

# 如果提示 Node.js 版本过低
node --version
nvm install 20 && nvm use 20
```

---

# 第二部分：核心功能详解

## 3. 基础使用

### 3.1 启动会话

Claude Code 以项目目录为工作上下文，启动方式灵活 [S001, S010]：

```bash
# 在项目根目录启动（推荐）
cd /path/to/your/project
claude

# 指定项目目录启动
claude --project /path/to/project

# 直接给出任务（非交互模式）
claude "解释这个项目的架构"

# 单次任务模式（执行完即退出）
claude -p "生成 README.md"

# 恢复上一次会话
claude --resume

# 从管道输入
cat error.log | claude "分析这个错误日志"
```

**最佳实践**：始终在项目根目录启动 Claude Code，这样它能自动发现 `CLAUDE.md` 配置文件并理解项目结构 [S001, S010]。`--resume` 参数允许恢复中断的长任务，这在大型重构等耗时场景中尤为重要 [S010]。

### 3.2 基本命令（斜线命令）

在交互会话中，Claude Code 支持一系列斜线命令来执行特定操作 [S009, S022]：

| 命令 | 功能 | 用法示例 |
|------|------|----------|
| `/help` | 显示帮助信息 | `/help` |
| `/memory` | 编辑项目 CLAUDE.md | `/memory` |
| `/compact` | 压缩对话历史 | `/compact` |
| `/clear` | 清除当前上下文 | `/clear` |
| `/vim` | 切换 Vim 编辑模式 | `/vim` |
| `/terminal-setup` | 配置终端显示 | `/terminal-setup` |
| `/commit` | 生成 Git commit | `/commit` |
| `/pr` | 创建 Pull Request | `/pr` |
| `/doctor` | 诊断系统状态 | `/doctor` |
| `/logout` | 退出登录 | `/logout` |
| `/cost` | 查看 Token 使用量 | `/cost` |

### 3.3 文件操作

Claude Code 提供四种核心文件操作工具，这是其"执行能力"的基石 [S003, S036]：

**Read（读取文件）**：读取文件内容并加载到上下文中。

```
> 阅读 src/index.ts 文件并解释其功能
```

**Write（写入文件）**：创建新文件或完全重写已有文件。

```
> 创建一个新的 src/utils/logger.ts 日志工具模块
```

**Edit（编辑文件）**：对已有文件进行精确的局部修改，只修改需要改变的部分。

```
> 在 src/app.ts 中添加错误处理中间件
```

**Glob（文件搜索）**：使用 glob 模式在项目中搜索文件 [S003]。

```
> 找到所有的测试文件
```

**工具调用的架构理解**：这四个工具构成了 Claude Code 的 **IO 层**。每次文件操作都有明确的权限确认机制（在 default 模式下），确保 AI 不会在未经用户允许的情况下修改文件。这种设计平衡了自动化效率与安全性 [S003]。

**文件操作的最佳实践** [S003]：
- 优先使用 Edit 而非 Write，以减少意外覆盖
- 使用 Glob 精确定位文件，避免不必要的全目录扫描
- 对关键文件修改前，先用 Read 确认当前状态

### 3.4 权限模式

Claude Code 提供四种权限模式，构成从"完全手动"到"完全自动"的控制频谱 [S019, S024]：

| 模式 | 读取文件 | 编辑文件 | 执行命令 | 适用场景 |
|------|---------|---------|---------|---------|
| `default` | 自动 | 需确认 | 需确认 | 日常开发，学习阶段 [S019] |
| `acceptEdits` | 自动 | 自动 | 需确认 | 信任代码修改，谨慎执行命令 [S019] |
| `plan` | 自动 | 禁止 | 禁止 | 分析和规划，不做实际修改 [S019] |
| `dangerously-skip-permissions` | 自动 | 自动 | 自动 | CI/CD、快速原型（谨慎使用）[S024] |

**模式选择决策矩阵：**

```
你是否在生产项目上工作？
├── 是 --> 你了解即将执行的任务吗？
│   ├── 了解 --> acceptEdits 模式
│   └── 不了解 --> plan 模式先规划
└── 否 --> 是 CI/CD 环境吗？
    ├── 是 --> dangerously-skip-permissions
    └── 否 --> default 模式（学习和原型开发）
```

配置方式：

```bash
# 启动时指定模式
claude --permission-mode acceptEdits

# 在 settings.json 中配置默认模式
# 位置: ~/.config/claude-code/settings.json
```

```json
{
  "permissions": {
    "mode": "acceptEdits"
  }
}
```

> **安全提醒**：`dangerously-skip-permissions` 模式会跳过所有确认，包括文件删除和任意命令执行。仅在你完全了解风险的情况下使用，且切勿在生产环境中启用 [S024]。

---

## 4. 工作模式

### 4.1 Default 默认模式

Default 模式是 Claude Code 的标准交互模式 [S019]。在此模式下：

- Claude 会在执行任何写操作之前请求确认
- 读取文件和分析代码不需要确认
- 适合处理重要项目和生产环境代码

**工作流程**：
1. 用户输入自然语言指令
2. Claude 分析需求，规划执行步骤
3. 对于读取操作，自动执行
4. 对于写入/执行操作，展示计划并等待确认
5. 用户确认后执行，展示结果

**适用场景**：日常开发、代码审查、学习探索。这是新手推荐使用的模式 [S003]。

### 4.2 Auto 自动模式（YOLO 模式）

Auto 模式让 Claude Code 获得更大的自主权，可以自动执行大多数操作而无需逐一确认 [S024]。

```bash
# 启动 Auto 模式
claude --dangerously-skip-permissions

# 或者在会话中通过 Shift+Tab 切换
```

**适用场景** [S024]：
- 快速原型开发：需要快速迭代，不在乎中间过程
- CI/CD 流水线：自动化场景中没有人工交互的可能
- 沙盒环境：在 Docker 容器等隔离环境中安全运行
- 一次性脚本编写

**Auto 模式的典型工作流** [S024]：

```
> 创建一个完整的 Express.js REST API，包含用户 CRUD 操作、JWT 认证、错误处理和单元测试
```

在 Auto 模式下，Claude 会自动完成项目初始化、依赖安装、源文件创建、测试编写和运行等全部步骤。

**风险控制建议**：
- 在 Git 仓库中使用，确保可以随时回退 [S003]
- 设置文件操作的白名单/黑名单
- 在隔离环境（Docker、VM）中运行
- 不要在包含敏感数据的目录中使用

### 4.3 Plan 计划模式

Plan 模式让 Claude 只进行分析和规划，不执行任何实际修改 [S019, S027]。

**设计理念**：Plan 模式体现了"先思考，后行动"的软件工程原则。在复杂任务中，先用 Plan 模式生成详细的实施方案，审核通过后再切换到执行模式，可以大幅减少返工 [S003]。

```bash
# 使用 Plan 模式
claude --permission-mode plan
```

```
> 我想把项目从 JavaScript 迁移到 TypeScript，请制定详细的迁移计划

# Claude 会输出：
# 1. 影响范围分析
# 2. 迁移步骤规划
# 3. 潜在风险点
# 4. 时间和工作量估算
# 但不会实际修改任何文件
```

### 4.4 模式切换技巧

在会话过程中，可以随时切换工作模式 [S024]：

- **Shift+Tab**：在 Auto/Default/Plan 三种模式之间循环切换
- 这允许你在规划阶段使用 Plan 模式，确认方案后切换到 Auto 模式执行

**模式组合策略（推荐工作流）**：

| 阶段 | 模式 | 目的 |
|------|------|------|
| 需求分析 | Plan | 理解代码库，生成分析报告 |
| 方案设计 | Plan | 输出实施方案，不修改代码 |
| 代码实现 | Default / acceptEdits | 按计划执行修改 |
| 快速迭代 | Auto (acceptEdits) | 信任修改，加速开发 |
| 代码审查 | Plan | 只读模式审查变更 |

---

## 5. 上下文管理

### 5.1 CLAUDE.md 核心概念

`CLAUDE.md` 是 Claude Code 最重要的配置机制，它相当于**项目的记忆文件**——告诉 Claude 关于你的项目的一切重要信息 [S004, S019, S027]。可以将其类比为"给 AI 的项目手册"——每次 Claude Code 启动时，会自动读取并加载 `CLAUDE.md` 的内容作为系统上下文。

### 5.2 三层内存架构

Claude Code 采用精心设计的三层内存体系，从组织到个人逐级细化 [S004, S030]：

```
┌─────────────────────────────────────┐
│  企业策略层 (Enterprise Policy)      │  ← 组织管理员配置
│  路径: 管理控制台设定                 │  ← 强制执行，不可覆盖
│  ~/.config/claude-code/CLAUDE.md    │
├─────────────────────────────────────┤
│  项目内存层 (Project Memory)         │  ← 团队共享规范
│  路径: {项目根}/CLAUDE.md           │  ← 提交到 Git，版本控制
│       {项目根}/.claude/rules/*.md   │  ← 模块化规则文件
├─────────────────────────────────────┤
│  用户内存层 (User Memory)            │  ← 个人偏好
│  路径: ~/.claude/CLAUDE.md          │  ← 跨项目生效
│       ~/.claude/rules/*.md         │  ← 个人规则
└─────────────────────────────────────┘
```

**加载优先级**（从高到低）[S004]：
1. 企业策略——组织级强制规则
2. 用户 CLAUDE.md——个人全局偏好
3. 项目 CLAUDE.md——团队共享规范
4. `.claude/rules/` 目录下的规则文件——模块化补充规则
5. 子目录 CLAUDE.md——特定目录的局部规则

**架构设计理念**：这种分层结构借鉴了 CSS 的级联优先级机制。企业策略类似于 `!important`，不可被下层覆盖；而项目和用户层可以互相补充。这使得大型团队可以在保持统一规范的同时，允许个人定制 [S004, S030]。

### 5.3 CLAUDE.md 编写最佳实践：8 条黄金法则

根据社区和官方推荐，编写 `CLAUDE.md` 应遵循以下 8 条黄金法则 [S027]：

**法则 1：保持简短精炼**

`CLAUDE.md` 的内容会占用上下文窗口，过长的配置会挤压实际工作的可用空间 [S027, S004]。

```markdown
<!-- 好的写法 -->
# 技术栈
- Next.js 15 + TypeScript + Tailwind CSS
- Prisma ORM + PostgreSQL
- Jest 用于测试

<!-- 避免的写法 -->
# 技术栈
本项目使用 Next.js 框架，版本号为 15，采用 App Router 架构。
前端样式使用 Tailwind CSS 进行处理......（冗长描述）
```

**法则 2：使用清晰的结构**

使用 Markdown 标题和列表来组织信息，便于 Claude 快速定位关键内容 [S027]。

**法则 3：避免冗余信息**

不要重复 Claude 已经能从代码中推断的信息（如 `package.json` 中已有的依赖列表）[S027]。

**法则 4：具体而非抽象**

```markdown
<!-- 好的写法 -->
- 函数命名使用 camelCase
- 组件文件使用 PascalCase.tsx
- 错误处理使用 Result<T, Error> 模式，不要用 try-catch
- API 响应格式: { data: T, error: string | null, code: number }

<!-- 避免的写法 -->
- 请遵循良好的命名规范
- 请注意错误处理
```

**法则 5：包含关键上下文**

记录那些 Claude 无法从代码中自动推断的信息，如架构决策背景、特殊的业务逻辑约束等 [S027]。

**法则 6：定期更新维护**

项目演进时同步更新 `CLAUDE.md`，保持其与实际代码的一致性 [S027]。

**法则 7：使用导入机制**

将大型配置拆分为多个文件，使用 `@import` 语法引用 [S027, S004]。

**法则 8：分离公私配置**

项目共享配置放在项目根目录的 `CLAUDE.md`，个人偏好放在 `~/.claude/CLAUDE.md` [S027]。

**推荐的 CLAUDE.md 模板** [S027, S028, S029]：

```markdown
# 项目概述
- 项目名称：MyApp
- 类型：全栈 Web 应用
- 技术栈：Next.js 15 + TypeScript + Prisma + PostgreSQL

# 项目结构
- src/app/ - Next.js App Router 页面
- src/components/ - React 组件
- src/lib/ - 工具函数和服务
- prisma/ - 数据库 schema 和 migrations

# 编码规范
- 使用 TypeScript strict 模式
- 组件使用函数式写法 + hooks
- 状态管理使用 Zustand
- API 错误统一使用 AppError 类

# 测试规范
- 单元测试使用 Jest + React Testing Library
- 测试文件命名：*.test.ts(x)
- 运行测试：npm test
- 运行单个文件测试：npx jest path/to/file.test.ts

# Git 规范
- commit 信息遵循 Conventional Commits
- 格式：type(scope): description
- 类型：feat / fix / docs / refactor / test / chore

# 常用命令
- npm run dev - 启动开发服务器
- npm run build - 构建生产版本
- npm run lint - 代码检查
- npx prisma migrate dev - 数据库迁移

# 导入额外文档
@docs/architecture.md
@docs/api-guidelines.md
```

### 5.4 导入继承机制

Claude Code 支持使用 `@import` 语法在 `CLAUDE.md` 中引用其他文件，实现配置的模块化管理 [S004, S030]：

```markdown
# CLAUDE.md 中的导入语法
@docs/architecture.md
@docs/coding-standards.md
@.claude/rules/security.md
```

**导入规则**：
- 支持相对路径和绝对路径 [S004]
- 递归导入最多支持 **5 层**嵌套 [S004, S030]
- 系统自动检测并避免循环引用 [S030]
- 被导入文件的内容会被展开到引用位置

### 5.5 模块化规则 (.claude/rules/)

除了 `CLAUDE.md`，Claude Code 还支持在 `.claude/rules/` 目录下创建模块化规则文件 [S004]：

```
.claude/
└── rules/
    ├── code-style.md          # 代码风格规则
    ├── testing.md              # 测试相关规则
    ├── security.md             # 安全规则
    ├── api-design.md           # API 设计规则
    └── git-workflow.md         # Git 工作流
```

规则文件支持 **Glob 模式匹配**，可以为特定类型的文件定义专属规则 [S004]：

```markdown
<!-- .claude/rules/react-components.md -->
<!-- globs: src/components/**/*.tsx -->

# React 组件规则
- 所有组件必须导出 Props 类型定义
- 使用 forwardRef 处理 ref 传递
- 必须包含 displayName
- Props 使用 interface 定义，不用 type
- 使用 memo 包裹纯展示组件
```

### 5.6 内存优化策略

**快速添加规则**：使用 `#` 快捷方式在对话中快速添加记忆 [S004, S030]。

```bash
# 在会话中输入 # 后跟规则内容
> # 所有新组件都使用 Tailwind CSS，不使用 styled-components
# 该规则会自动保存到 CLAUDE.md
```

**使用 `/memory` 命令**：直接打开编辑器修改 `CLAUDE.md` [S004, S030]。

```bash
> /memory
# 会打开默认编辑器编辑 CLAUDE.md
```

**内存查找机制**：Claude Code 启动时会从当前目录向上递归查找 CLAUDE.md 文件，直到文件系统根目录 [S004, S030, S031]。这意味着你可以在 monorepo 中为每个子包设置独立的规则文件。

**内存清理**：当项目发生重大变更时，清理过时的记忆 [S033]。

```bash
# 重置项目记忆
> /clear

# 或手动编辑 CLAUDE.md 删除过时内容
> /memory
```

---

# 第三部分：高级特性

## 6. Skills 技能系统

### 6.1 概念与架构

Skills（技能）是 Claude Code 的**知识扩展层**，本质上是一组结构化的 Markdown 指令文件，用于教会 Claude 在特定场景下应该如何思考和行动 [S006, S007, S008]。

**架构定位**：如果将 Claude Code 比作一个操作系统，那么 Skills 就是安装在其上的"应用程序知识库"。它不提供新的工具能力（那是 MCP 的职责），而是提供"如何使用现有工具完成特定任务"的专家知识 [S017]。

```
Claude Code 扩展架构
├── Skills（知识层）    --> "知道怎么做" --> Markdown 指令文件
├── MCP（工具层）       --> "能做什么"   --> 外部工具/API 连接
├── Hooks（流程层）     --> "何时触发"   --> 事件驱动的自动化
└── Subagents（协作层）  --> "谁来做"    --> 任务分派与并行执行
```

**Skills vs MCP vs Hooks 完整对比** [S017]：

| 维度 | Skills | MCP Server | Hooks |
|------|--------|------------|-------|
| **本质** | 知识/工作流指令 | 外部工具连接 | 事件触发脚本 |
| **作用** | 教 Claude "怎么做" | 让 Claude "能做什么" | 在 Claude 动作前后"自动执行" |
| **形式** | Markdown 文件 | JSON 配置 + 服务进程 | Shell 脚本/可执行文件 |
| **Token 消耗** | 按需加载，较低 | 工具描述常驻，较高 | 无 token 消耗 |
| **可移植性** | 高（纯文本） | 中（需安装 Server） | 中（依赖系统环境） |
| **开发门槛** | 低（写 Markdown） | 中（配置 JSON + 依赖） | 中（编写脚本） |

**工作原理**：当你通过斜线命令调用一个 Skill 时，Claude Code 会加载对应的 `SKILL.md` 文件到上下文中，从而获得该领域的专业知识和操作流程 [S007, S008]。

### 6.2 官方 Skills 推荐

以下是社区和官方推荐的高质量 Skills [S021]：

**Superpowers（超能力）**（1.6 万+ Star）
- 功能：增强 Claude 的系统级能力，包括更好的规划、代码审查、重构策略
- 安装：`/install-skill https://github.com/anthropics/claude-code-superpowers`
- 适用：所有项目，建议作为基础 Skill 安装

**Planning-with-files（文件规划）**
- 功能：使用文件系统进行持久化的任务规划和进度追踪
- 适用：大型功能开发、跨多会话的复杂任务

**UI UX Pro Max**
- 功能：前端 UI/UX 设计最佳实践，响应式布局、无障碍性
- 适用：前端项目，需要生成高质量 UI 代码的场景

**seo-review / content-creator**
- 功能：SEO 优化审查和内容生成
- 适用：内容驱动的 Web 项目

**skill-prompt-generator**
- 功能：辅助创建新的自定义 Skill
- 适用：需要开发自己的 Skill 的进阶用户

### 6.3 Skills 安装与管理

安装 Skill 的方式 [S021, S012]：

```bash
# 方式一：使用斜线命令安装
> /install-skill <skill-url>

# 方式二：手动安装（将 Skill 文件放入指定目录）
mkdir -p ~/.claude/skills/my-custom-skill/
cp SKILL.md ~/.claude/skills/my-custom-skill/

# 查看已安装的 Skills
> /skills

# 使用特定 Skill
> /skill:code-review
```

Skills 的目录结构 [S007]：

```
~/.claude/skills/
├── superpowers/
│   ├── SKILL.md
│   └── resources/
├── planning-with-files/
│   └── SKILL.md
└── my-custom-skill/
    ├── SKILL.md
    ├── resources/
    │   ├── templates/
    │   └── examples/
    └── README.md
```

### 6.4 自定义 Skills 开发

创建自定义 Skill 的核心是编写 `SKILL.md` 文件 [S006, S007]。以下是一个完整的实战示例：

**示例：创建代码审查 Skill**

```markdown
# SKILL.md - Code Review Skill

## 描述
对代码进行全面审查，重点关注安全性、性能和可维护性。

## 触发条件
当用户请求代码审查或使用 /review 命令时激活。

## 审查流程

### 步骤 1：理解上下文
- 阅读被审查的文件和相关依赖
- 了解项目的编码规范（从 CLAUDE.md 获取）

### 步骤 2：安全检查
- [ ] 检查是否有硬编码的密钥或凭证
- [ ] 检查 SQL 注入风险
- [ ] 检查 XSS 漏洞
- [ ] 检查权限验证逻辑

### 步骤 3：性能审查
- [ ] 检查 N+1 查询问题
- [ ] 检查不必要的重渲染
- [ ] 检查内存泄漏风险
- [ ] 大数据集是否有分页处理

### 步骤 4：代码质量
- [ ] 命名是否清晰
- [ ] 函数是否过长（>50行警告，圈复杂度>10需警告）
- [ ] 错误处理是否完整
- [ ] 类型定义是否准确
- [ ] 是否遵循单一职责原则

## 输出格式
以 Markdown 表格输出审查结果：
| 严重级别 | 位置 | 问题描述 | 建议修改 |

## 约束
- 不要自动修改代码，只提供建议
- 始终解释问题的原因，而不仅仅指出问题
- 标注问题所在的文件名和行号
```

**Skills 开发最佳实践** [S006, S007]：

1. **单一职责原则**：每个 Skill 聚焦一个特定任务，不要做"万能 Skill"
2. **步骤分解**：将复杂任务拆分为清晰的步骤序列
3. **明确的输出格式**：定义结构化的输出模板，便于后续处理
4. **包含约束条件**：设定 Skill 的行为边界
5. **可测试性**：Skill 的效果应该可以通过具体案例验证
6. **提供示例**：在 `resources/examples/` 目录下放置参考示例

### 6.5 实战案例：测试用例生成 Skill

```markdown
# SKILL.md - Test Generator

## 描述
为指定的源代码文件自动生成全面的单元测试。

## 流程

### 1. 分析源文件
- 识别所有导出的函数/类/组件
- 分析参数类型和返回值
- 识别边界条件和异常路径

### 2. 生成测试用例
对每个导出的函数，生成以下类型的测试：
- Happy path（正常路径）
- Edge cases（边界条件）
- Error cases（错误场景）
- Null/Undefined（空值测试）
- Type validation（类型验证）

### 3. 测试规范
- 使用项目已配置的测试框架（从 package.json 或 CLAUDE.md 获取）
- 默认使用 Vitest
- 使用 describe/it 结构
- 每个 it 块只测试一个行为
- 使用 AAA 模式 (Arrange-Act-Assert)
- Mock 外部依赖

### 4. 输出与验证
- 测试文件放置在源文件同目录的 __tests__/ 下
- 文件名: {source}.test.{ext}
- 运行生成的测试确保通过
- 检查覆盖率
```

---

## 7. MCP Server 集成

### 7.1 MCP 协议介绍

MCP（Model Context Protocol，模型上下文协议）是 Anthropic 提出的一种标准化协议，用于连接 AI 模型与外部工具和数据源 [S008, S015]。它被比喻为 **"AI 的 USB-C"**——一个通用的接口标准 [S018]。

**MCP 在 Claude Code 中的角色**：

```
用户请求 --> Claude 推理 --> 选择 MCP 工具 --> 调用外部服务 --> 处理结果 --> 返回用户
                              |
                    +-----------------+
                    | MCP Server 1    | <-- 数据库查询
                    | MCP Server 2    | <-- 网页搜索
                    | MCP Server 3    | <-- 文件系统
                    | MCP Server N    | <-- 自定义服务
                    +-----------------+
```

**Skills vs MCP 的协作关系**：Skills 告诉 Claude "如何完成任务的知识"，MCP 给 Claude "实际执行任务的工具"。例如，一个数据库迁移 Skill 知道迁移的最佳实践步骤，而 Supabase MCP Server 提供了实际执行 SQL 的能力。两者组合使用效果最佳 [S008]。

### 7.2 配置方法

MCP Server 通过 JSON 配置文件进行管理 [S015, S018]。

**配置文件位置**：

```bash
# 项目级配置（推荐，团队共享）
<project-root>/.claude/mcp.json

# 用户级配置（个人偏好，跨项目）
~/.config/claude-code/mcp.json
```

**基础配置结构**：

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/server-name"],
      "env": {
        "API_KEY": "${API_KEY}"
      }
    }
  }
}
```

也可以通过命令行直接添加 [S015]：

```bash
# 使用 claude mcp 命令管理 MCP Server
claude mcp add brave-search npx -y @anthropic-ai/mcp-server-brave-search
```

> **安全提示**：将 API 密钥存储在环境变量中，使用 `${VAR}` 语法引用，不要在配置文件中硬编码密钥 [S015]。

### 7.3 常用 MCP Servers

| MCP Server | 功能 | 典型用途 | 配置复杂度 |
|-----------|------|---------|-----------|
| **Brave Search** | 网页搜索 | 查询最新技术文档、API 参考 [S018] | 低 |
| **GitHub** | 代码仓库 | PR 管理、Issue 处理、代码搜索 [S015] | 低 |
| **Supabase** | 数据库操作 | 查询/修改数据、管理表结构 [S015] | 中 |
| **Figma** | 设计文件 | 提取设计稿尺寸、颜色、组件信息 [S015] | 中 |
| **Playwright** | 浏览器自动化 | E2E 测试、网页截图、表单填写 [S015] | 中 |
| **Filesystem** | 增强文件操作 | 跨目录文件管理、批量操作 | 低 |
| **Sentry** | 错误监控 | 查看和分析生产环境错误 | 中 |

**MCP Server 选择决策**：

```
你需要什么额外能力？
├── 搜索互联网信息  --> Brave Search
├── 操作数据库     --> Supabase / PostgreSQL MCP
├── 管理 GitHub    --> GitHub MCP
├── 从设计稿开发   --> Figma MCP
├── 浏览器测试     --> Playwright MCP
├── 监控生产错误   --> Sentry MCP
└── 自定义需求     --> 开发自定义 MCP Server
```

**配置示例合集**：

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${BRAVE_API_KEY}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "supabase": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-supabase"],
      "env": {
        "SUPABASE_URL": "${SUPABASE_URL}",
        "SUPABASE_KEY": "${SUPABASE_KEY}"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-playwright"]
    },
    "figma": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-figma"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "${FIGMA_ACCESS_TOKEN}"
      }
    }
  }
}
```

### 7.4 上下文优化（Token 消耗问题）

MCP Server 是一把双刃剑。每个启用的 MCP Server 都会将其工具描述加载到上下文中，**即使你没有使用它**。根据实测数据，某些配置可能消耗超过 **66,000 tokens** 仅用于工具描述 [S016]。

**Token 消耗实例** [S016]：

| MCP Server | 大致 Token 消耗 |
|-----------|----------------|
| Brave Search | ~2,000 tokens |
| GitHub | ~5,000 tokens |
| Supabase | ~8,000 tokens |
| 多个 Server 叠加 | 可达 66,000+ tokens |

**优化策略** [S016]：

1. **按需启用**：只在需要时激活对应的 MCP Server，不要一次性启用所有 Server

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-key"
      },
      "disabled": true
    }
  }
}
```

2. **使用 `/doctor` 和 `/cost` 命令**监控上下文使用情况 [S016]

3. **项目级配置**：为不同项目配置不同的 MCP Server 集合，避免全局加载过多

4. **定期清理**：移除不再使用的 MCP Server 配置

### 7.5 MCP 实战案例

**案例一：使用 GitHub MCP 自动化 PR 审查**

```bash
# 确保 GitHub MCP 已配置
claude

> 请审查 PR #42 的所有变更，重点检查安全问题和性能影响，
> 然后在 PR 上添加审查评论
```

Claude Code 会通过 GitHub MCP Server 获取 PR 变更、分析影响、自动提交 Review 评论。

**案例二：使用 Supabase MCP 进行数据库驱动开发**

```
> 查看 users 表的结构
> 添加一个 email_verified 布尔字段，默认值为 false
> 生成对应的 TypeScript 类型定义
> 编写一个用户注册的 API endpoint，包含邮箱验证逻辑
```

Claude 会调用 Supabase MCP 执行数据库操作，并基于实际的表结构生成准确的代码 [S015]。

---

## 8. Hooks 钩子系统

### 8.1 工作原理

Hooks（钩子）是 Claude Code 的**流程自动化层**，采用事件驱动机制，允许在 Claude 执行特定操作的前后自动运行自定义脚本 [S009, S022]。

**事件模型**：

```
用户输入 --> [PreToolUse Hook] --> Claude 工具调用 --> [PostToolUse Hook] --> 结果返回
                                                                         |
                                                              [Notification Hook]
                                                                         |
                                                                  [Stop Hook]
```

**支持的 Hook 事件类型** [S009, S022]：

| 事件 | 触发时机 | 典型用途 |
|------|----------|----------|
| `PreToolUse` | 工具调用之前 | 参数验证、权限检查、格式化 |
| `PostToolUse` | 工具调用之后 | 结果处理、日志记录、通知 |
| `Notification` | Claude 发送通知时 | 自定义提醒方式 |
| `Stop` | Claude 完成任务时 | 后处理、统计、清理 |

### 8.2 常用钩子场景

#### 场景一：代码格式化（保存后自动格式化）

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\"",
        "description": "文件修改后自动格式化"
      }
    ]
  }
}
```

每当 Claude 写入或编辑文件后，自动使用 Prettier 格式化代码 [S009]。

#### 场景二：测试自动运行

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "bash -c 'if [[ \"$CLAUDE_FILE_PATH\" == *.test.* ]]; then npx jest \"$CLAUDE_FILE_PATH\" --no-coverage; fi'",
        "description": "测试文件修改后自动运行"
      }
    ]
  }
}
```

当 Claude 修改测试文件时，自动运行对应的测试 [S009]。

#### 场景三：提交前检查

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit.*)",
        "command": "npm run lint && npm run test:unit",
        "description": "提交前运行 lint 和单元测试"
      }
    ]
  }
}
```

在 Claude 执行 `git commit` 之前，自动运行 lint 和测试检查 [S009]。

#### 场景四：通知推送

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "osascript -e 'display notification \"Claude 任务完成\" with title \"Claude Code\"'",
        "description": "任务完成后发送 macOS 通知"
      }
    ]
  }
}
```

当 Claude 完成任务时，通过系统通知提醒你（macOS）。

### 8.3 自定义钩子开发

Hook 配置文件位于项目的 `.claude/settings.json` 或全局配置中 [S022]：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "ToolName",
        "command": "path/to/script.sh"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "node path/to/post-process.js"
      }
    ],
    "Stop": [
      {
        "command": "bash -c 'echo \"Task completed at $(date)\" >> ~/.claude/activity.log'"
      }
    ]
  }
}
```

**Hook 脚本可用的环境变量**：

| 变量 | 说明 |
|------|------|
| `$CLAUDE_FILE_PATH` | 当前操作的文件路径 |
| `$CLAUDE_TOOL_INPUT` | 工具的输入参数 |
| `$CLAUDE_TOOL_OUTPUT` | 工具的输出结果（PostToolUse） |

**自定义检查脚本示例**：

```bash
#!/bin/bash
# .claude/hooks/pre-commit-check.sh
# 提交前的自定义检查脚本

set -e

echo "Running pre-commit checks..."

# 检查是否有 console.log 残留
if grep -r "console.log" src/ --include="*.ts" --include="*.tsx"; then
    echo "ERROR: Found console.log statements in source code"
    exit 1
fi

# 检查是否有 TODO 标记
TODO_COUNT=$(grep -r "TODO" src/ --include="*.ts" --include="*.tsx" -c || echo "0")
if [ "$TODO_COUNT" -gt "0" ]; then
    echo "WARNING: Found $TODO_COUNT TODO comments"
fi

echo "Pre-commit checks passed!"
exit 0
```

### 8.4 Hooks 最佳实践

| 原则 | 说明 | 原因 |
|------|------|------|
| **保持轻量** | Hook 脚本应在数秒内完成 | 避免阻塞 Claude 的工作流 [S009] |
| **错误宽容** | 非关键 Hook 不应阻断流程 | 使用 `\|\| true` 避免意外中断 [S009] |
| **日志记录** | 记录 Hook 的执行状态 | 便于调试和审计 [S009] |
| **幂等性** | Hook 重复执行结果一致 | 避免副作用累积 |
| **安全意识** | 不在 Hook 中硬编码密钥 | 使用环境变量传递敏感信息 |
| **精确匹配** | 通过正则表达式精确控制触发范围 | 避免误触发 |

---

## 9. Subagents 子代理

### 9.1 协作模式

Subagents（子代理）是 Claude Code 的**任务协作层**，允许主 Claude 实例将子任务分派给独立的 Claude 子进程处理 [S017, S020, S023]。这种设计模式借鉴了操作系统中的进程模型——主进程负责调度，子进程负责执行。

与 Skills 和 MCP 的本质区别在于：Skills 注入知识，MCP 连接工具，而 **Subagents 提供并行执行能力** [S017]。

**Subagents 的架构定位**：

| 层级 | 角色 | 工作方式 |
|------|------|---------|
| **主 Agent** | 任务编排者 | 分析需求、拆分任务、汇总结果 |
| **Sub Agent** | 任务执行者 | 独立上下文、聚焦单一任务 |
| **Task Tool** | 分派机制 | 主 Agent 用来创建和管理子任务 |

### 9.2 并行任务处理（Task Tool）

Claude Code 内置的 **Task Tool** 允许将任务分派给子代理 [S020, S036]：

```
主 Agent 接收任务: "为这个项目添加用户认证系统"
    |
    +-- Task 1: "分析现有的用户模型和数据库结构"
    +-- Task 2: "设计 JWT 认证流程"
    +-- Task 3: "编写认证中间件"
    +-- Task 4: "生成相关的单元测试"
```

**任务分解策略** [S023]：

1. **上下文隔离原则**：每个子任务应该有独立的、最小化的上下文。子 Agent 不会继承主 Agent 的完整对话历史，这既减少了 token 消耗，也避免了无关信息干扰 [S020, S023]。

2. **任务粒度控制**：子任务应足够具体，能在单次交互中完成。过大的子任务会导致子 Agent 需要过多上下文；过小的子任务会增加调度开销。

3. **按维度分解**：
   - **按功能模块分解**：不同模块分配给不同子代理
   - **按阶段分解**：分析阶段和实现阶段分别处理
   - **按文件类型分解**：前端和后端代码分开处理

4. **结果聚合**：主 Agent 负责收集所有子任务的结果，进行冲突检测和整合。

### 9.3 多 Claude 协作工作流

除了自动的子代理分派，你还可以手动开启多个 Claude Code 终端实现协作 [S020, S039]：

#### 工作流一：编码与验证分离

```bash
# 终端 1：编码 Claude（负责编写代码）
claude
> 实现用户注册功能，包含邮箱验证

# 终端 2：审查 Claude（负责代码审查）
claude
> 监控 src/ 目录的变更，对每个新增/修改的文件进行代码审查

# 终端 3：测试 Claude（负责测试验证）
claude
> 监控 src/ 目录的变更，为新增的功能编写并运行测试
```

#### 工作流二：测试驱动协作（TDD 双 Claude 模式）

```bash
# Claude 1（测试编写者）
claude
> 为用户认证模块编写完整的测试套件，包含所有边界情况
# 等待测试编写完成...

# Claude 2（功能实现者）
claude
> 阅读 tests/ 目录下的认证测试，实现所有测试用例要求的功能，
> 确保所有测试通过
```

这种模式完美模拟了 TDD 流程：先由 Claude 1 定义"合约"（测试），再由 Claude 2 实现满足合约的代码 [S020, S039]。

#### 工作流三：前后端并行开发

```bash
# 终端 1 (后端 Claude)
claude
> 实现 RESTful API：用户 CRUD 和认证接口
> 生成 OpenAPI 规范文档

# 终端 2 (前端 Claude)
claude
> 根据 docs/api-spec.yaml 实现前端页面和 API 调用
```

**并行开发注意事项** [S020, S023]：

| 风险 | 应对策略 |
|------|---------|
| 文件冲突 | 明确每个 Claude 实例的文件操作范围，避免同时修改同一文件 |
| 接口不一致 | 先定义接口契约（API 规范、TypeScript 类型），再并行开发 |
| 上下文发散 | 使用共享的 CLAUDE.md 确保所有实例遵循统一规范 |
| 资源竞争 | 避免多个实例同时运行数据库迁移等排他操作 |

### 9.4 企业实践：得物技术的经验

得物技术团队在大规模 AI 编程实践中，总结了以下 Subagent 协作经验 [S023]：

**对话流设计方法论**：
- 将复杂业务需求拆解为多轮对话流，每轮聚焦一个明确的子目标
- 通过上下文传递机制保持对话连贯性

**子代理系统设计思考**：
- 为不同职责的 Agent 配置专属的 CLAUDE.md 规则
- 按领域划分子代理职责（如前端 Agent、后端 Agent、测试 Agent）
- 建立 Agent 间的通信协议（通过文件系统或共享目录）
- 在代码审查环节引入独立的"审查 Agent"，实现人机混合审查

**质量门禁与经验沉淀**：
- 在子代理输出和主流程之间设置检查点，确保每个阶段的产出质量
- 将成功的协作模式固化为 Skills 或 CLAUDE.md 配置，实现团队复用
- 企业级 CLAUDE.md 作为基线规范，确保所有开发者的 Claude 行为一致
- 定期同步 CLAUDE.md 更新，纳入团队的代码评审流程

---

# 第四部分：工作流优化

## 10. 高效工作流

### 10.1 探索-规划-编码流程

Anthropic 官方推荐的三阶段开发流程 [S003, S012, S019]：

```
+----------+     +----------+     +----------+
|  Explore  | --> |   Plan   | --> |   Code   |
|  探索阶段  |     |  规划阶段  |     |  编码阶段  |
+----------+     +----------+     +----------+
```

**探索阶段 (Explore)**：

```
> 请分析这个项目的整体架构，重点关注：
> 1. 目录结构和模块划分
> 2. 核心数据流
> 3. 外部依赖和集成点
> 4. 现有的测试覆盖情况
```

在这个阶段，让 Claude 充分理解代码库，识别关键文件和依赖关系。推荐使用 Plan 模式，确保只读不写 [S003, S019]。

**规划阶段 (Plan)**：

```
> 基于你对项目的理解，制定以下任务的实施计划：
> [描述你要实现的功能]
>
> 请包含：
> 1. 需要修改的文件列表
> 2. 每个文件的具体修改内容
> 3. 潜在的风险和注意事项
> 4. 测试策略
```

此阶段使用 Plan 模式，让 Claude 输出完整的方案而不执行 [S019]。

**编码阶段 (Code)**：

```
> 按照刚才的计划开始实施，先从 [最核心的部分] 开始
```

确认规划方案后，切换到 Default 或 Auto 模式开始执行。建议每完成一个步骤就提交一次，便于回退 [S003]。

**三阶段工作流总结**：

| 阶段 | 推荐模式 | 主要操作 | 产出物 |
|------|---------|---------|--------|
| 探索 | Plan | Read, Glob, Grep | 架构理解、依赖图谱 |
| 规划 | Plan | 推理、分析 | 实施计划、风险评估 |
| 编码 | Default/acceptEdits | Write, Edit, Bash | 代码、测试、文档 |

### 10.2 TDD 测试驱动开发

Claude Code 非常适合 TDD 流程 [S003, S012, S019]：

**Red-Green-Refactor 循环**：

```bash
# 第 1 步 (Red)：编写失败的测试
> 为 calculateDiscount 函数编写测试用例，考虑以下场景：
> - 普通用户 10% 折扣
> - VIP 用户 20% 折扣
> - 折扣金额不超过 100 元
> - 负数金额抛出错误

# 运行测试确认失败
> 运行测试确认这些测试用例目前都会失败

# 第 2 步 (Green)：编写最小通过代码
> 现在实现 calculateDiscount 函数，使所有测试通过

# 第 3 步 (Refactor)：优化代码
> 测试全部通过了，请重构 calculateDiscount 的实现，
> 提升可读性，同时确保测试仍然通过
```

**Claude 辅助 TDD 的最佳实践** [S003, S012]：

- 在 `CLAUDE.md` 中明确测试框架和规范
- 让 Claude 先读懂已有测试的模式和风格
- 测试文件和源文件一起修改，保持同步
- 每次修改后立即运行测试验证
- 利用 `claude --resume` 保持 TDD 循环的上下文连续性

### 10.3 Git 集成最佳实践

Claude Code 提供深度的 Git 集成 [S009]：

#### 自动生成 commit 信息

```bash
# 使用 /commit 命令
> /commit

# Claude 会自动分析变更内容，生成语义化的 commit message：
# feat(auth): add JWT-based user authentication
#
# - Implement UserService with createUser/login methods
# - Add JWT token generation and validation middleware
# - Create auth routes with proper error handling
```

#### 一键创建 PR

```bash
> /pr

# Claude 会自动：
# 1. 推送当前分支到远程
# 2. 分析所有 commit 的变更
# 3. 生成 PR 标题和描述
# 4. 通过 gh CLI 创建 PR
```

#### 根据 Issue 修复

```bash
> 修复 GitHub Issue #123

# Claude 会：
# 1. 读取 Issue 内容
# 2. 分析相关代码
# 3. 实施修复
# 4. 编写测试
# 5. 提交代码
```

### 10.4 CI/CD 自动化

Claude Code 可以在 CI/CD 流水线中以 **Headless 模式** 运行 [S012]：

**GitHub Actions 集成示例**：

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "审查这个 PR 的所有变更，检查安全性和代码质量问题" \
            --dangerously-skip-permissions
```

**CI/CD 集成场景**：

| 场景 | Claude 任务 | 触发时机 |
|------|------------|---------|
| 代码审查 | 自动审查 PR 中的代码变更 | Pull Request 创建时 |
| 测试生成 | 为新增代码生成缺失的测试 | 代码推送时 |
| 文档更新 | 根据代码变更更新文档 | 代码合并后 |
| 安全扫描 | 检查安全漏洞和敏感信息 | 每次提交时 |
| 变更日志 | 自动生成 CHANGELOG | 发布版本时 |

---

## 11. 效率提升技巧

### 11.1 快捷键与命令

掌握这些快捷键能显著提升操作效率 [S019, S024, S004, S030]：

| 快捷键 | 功能 | 使用场景 |
|--------|------|----------|
| **Esc Esc** | 编辑/重发上一条消息 | 修改上一个提示词 |
| **Shift+Tab** | 在 Auto/Default/Plan 模式间切换 | 灵活切换工作模式 |
| **Shift+Enter** | 换行（不发送） | 输入多行内容 |
| **#** | 快速添加记忆到 CLAUDE.md | 随时保存项目规则 |
| **Tab** | 自动补全/接受建议 | 命令和路径补全 |
| **Ctrl+C** | 中断当前操作 | 停止正在执行的任务 |
| **Ctrl+D** | 退出会话 | 结束当前 Claude Code 会话 |
| **Ctrl+L** | 清屏 | 整理终端显示 |

**实用示例**：

```bash
# 使用 Esc Esc 修改上一条消息
# 如果上一条提示不够精确，按 Esc Esc 可以编辑并重新发送

# 使用 # 快速保存规则
> # 所有 API 路由必须包含版本前缀 /api/v1/
# 这条规则会自动保存到 CLAUDE.md

# 使用 Shift+Enter 输入多行内容
> 请创建一个新的 API 端点：[Shift+Enter]
> - 路径：/api/v1/users[Shift+Enter]
> - 方法：GET[Shift+Enter]
> - 支持分页和搜索
```

### 11.2 IDE 集成

#### VS Code 集成

Claude Code 提供 VS Code 扩展支持 [S012, S022]：

```bash
# 安装 VS Code 扩展
# 方法 1：在 VS Code 扩展市场搜索 "Claude Code"
# 方法 2：命令行安装
code --install-extension anthropic.claude-code
```

配置 VS Code 终端以获得最佳体验：

```json
// settings.json
{
  "terminal.integrated.fontFamily": "MesloLGS NF",
  "terminal.integrated.fontSize": 14,
  "terminal.integrated.scrollback": 10000,
  "terminal.integrated.enableImages": true,
  "terminal.integrated.env.osx": {
    "ANTHROPIC_API_KEY": "${env:ANTHROPIC_API_KEY}"
  }
}
```

**VS Code 使用技巧**：
- 在 VS Code 终端中运行 Claude Code，可以同时利用 IDE 的文件树和编辑器预览变更
- 使用 VS Code 的分割终端功能，一个面板运行 Claude，另一个面板运行测试
- 通过 VS Code 的 Source Control 面板直观查看 Claude 的文件变更

#### JetBrains IDEs

JetBrains 系列 IDE（IntelliJ IDEA、PyCharm、WebStorm 等）支持在内置终端中使用 Claude Code [S012, S022]：

```bash
# 在 JetBrains 终端中直接启动
claude
```

**WSL2 注意事项**：如果在 Windows + WSL2 环境下使用 JetBrains IDE，需要注意网络模式配置 [S005]：

```bash
# 确保 JetBrains 的 WSL 集成使用正确的网络模式
# 在 IDE 设置中：Tools > Terminal > Shell Path
# 设置为: wsl.exe -d Ubuntu

# 在 WSL2 配置中设置 mirrored 网络模式
# 编辑 .wslconfig
[wsl2]
networkingMode=mirrored
```

### 11.3 终端配置优化

**安装 Nerd Fonts**：Claude Code 使用特殊字符显示状态图标，安装 Nerd Fonts 可以获得完整的视觉体验 [S019]。

```bash
# macOS
brew install --cask font-meslo-lg-nerd-font

# 使用 /terminal-setup 命令自动配置
claude
> /terminal-setup
```

**Shell 别名设置** [S037, S038]：

```bash
# 在 ~/.zshrc 或 ~/.bashrc 中添加
alias cc="claude"
alias ccr="claude --resume"
alias ccp="claude --permission-mode plan"
alias cca="claude --dangerously-skip-permissions"

# 快速审查 PR
alias ccpr="claude '/pr'"

# 快速提交
alias cccommit="claude '/commit'"

# 快速在当前目录启动
alias ccd="claude --project $(pwd)"

# 设置默认权限模式
export CLAUDE_PERMISSION_MODE="acceptEdits"
```

### 11.4 批量操作技巧

**多文件批量修改** [S019, S037]：

```
> 在所有 .ts 文件中，将 console.log 替换为使用 logger.info
```

**批量重构** [S019]：

```
> 将 src/components/ 目录下所有 Class 组件重构为函数式组件，
> 保持接口不变并确保测试通过
```

**项目级搜索替换** [S037]：

```
> 项目中所有使用 moment.js 的地方迁移到 dayjs，
> 注意处理 API 差异和时区问题
```

Claude Code 的 200K 上下文窗口使其能够理解跨文件的依赖关系，在批量修改时保持一致性 [S002, S026]。

---

## 12. 性能优化

### 12.1 上下文窗口管理

Claude Code 的 200K token 上下文窗口虽然很大，但在复杂项目中仍然需要谨慎管理 [S002, S026]：

**上下文消耗来源分析**：

| 消耗来源 | 估算占比 | 优化空间 |
|---------|---------|---------|
| 系统提示词 | 5-10% | 低（固定消耗） |
| CLAUDE.md 内容 | 2-5% | 中（精简内容） |
| MCP 工具描述 | 10-30% | 高（按需加载） |
| 对话历史 | 30-50% | 高（定期清理） |
| 文件内容 | 20-40% | 高（精确读取） |

**管理策略**：

```bash
# 压缩对话历史（保留关键信息，删除冗余部分）
> /compact

# 完全重置上下文
> /clear

# 查看当前上下文使用情况
> /cost

# 使用 /doctor 进行系统诊断
> /doctor
```

**上下文管理原则**：
- 单个会话专注于一个任务，避免在一个长会话中处理多个不相关任务
- 及时使用 `/compact` 压缩不再需要的对话历史
- 大型文件不要完整加载，指定需要的行范围
- 在长会话中定期使用 `/cost` 监控 token 消耗

### 12.2 Token 使用优化

减少 Token 消耗不仅能降低成本，还能提高响应速度 [S003, S016]：

**减少不必要的文件读取**：

```
# 好的做法：指定需要查看的范围
> 阅读 src/services/auth.ts 中的 login 函数
> 读取 src/auth/jwt.ts 的第 20-50 行

# 避免的做法：加载整个大文件或盲目搜索
> 阅读 src/services/auth.ts 的全部内容
> 找到项目中处理 JWT 的代码（可能导致读取大量文件）
```

**使用 Glob 精确匹配** [S003]：

```
# 好的做法：精确匹配
> 找到 src/components/ 下所有以 Auth 开头的 .tsx 文件

# 避免的做法：模糊搜索
> 找到所有和认证相关的文件
```

**提示词优化**：
- 指令要具体、明确，减少 Claude 的"试探性"操作
- 复杂任务分步骤指示，而非一次性描述全部需求
- 引用具体的文件路径和函数名，减少搜索开销

### 12.3 缓存策略

Claude Code 具有一定的内部缓存机制 [S037]：

- **文件内容缓存**：已读取的文件在会话内有缓存，重复引用不会产生额外读取
- **工具结果缓存**：某些工具的调用结果会被缓存，减少重复调用
- **会话恢复**：使用 `--resume` 恢复的会话保留了之前的上下文缓存

**最佳实践**：

```
# 在会话开始时一次性加载关键文件
> 阅读以下文件：src/config.ts, src/types.ts, src/constants.ts
# 后续引用这些文件时不会再消耗 Token
```

**结果缓存**：对于耗时的分析操作，将结果保存到文件中避免重复计算 [S037]：

```
> 分析项目的依赖关系并将结果保存到 docs/dependencies.md
# 后续只需引用这个文件即可
```

### 12.4 响应速度提升

影响 Claude Code 响应速度的主要因素 [S033, S034]：

| 因素 | 影响程度 | 优化方法 |
|------|---------|---------|
| 网络延迟 | 高 | 使用低延迟网络，配置代理 |
| 上下文大小 | 高 | 控制上下文窗口使用，定期压缩 |
| MCP Server 数量 | 中 | 按需启用，禁用闲置 Server |
| 并发请求 | 中 | 避免多个 Claude 实例同时发大量请求 |
| 系统资源 | 低 | 确保足够的 CPU 和内存 |

**实用技巧**：

```bash
# 使用 /compact 压缩上下文提速
/compact

# 使用 -p 单次模式执行简单任务（避免会话开销）
claude -p "将 config.ts 中的端口改为 8080"

# 如果使用代理
export HTTPS_PROXY="http://proxy:port"
```

---

# 第五部分：问题解决

## 13. 常见问题与解决方案

### 13.1 安装类问题

#### 问题：命令找不到 (command not found)

**原因**：安装目录未加入 PATH 环境变量 [S032, S033]。

**解决方案**：

```bash
# 检查安装位置
which claude
ls ~/.local/bin/claude

# 将安装目录添加到 PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 也可检查 npm 全局安装路径
npm config get prefix
# 确认输出路径在 PATH 中

# 验证
claude --version
```

#### 问题：Node.js 版本不兼容

**原因**：系统安装的 Node.js 版本低于 18 [S033, S034]。

**解决方案**：

```bash
# 检查当前版本
node --version

# 使用 nvm 升级
nvm install 20
nvm alias default 20

# 验证
node --version
# 应输出 v20.x.x
```

#### 问题：WSL 路径冲突

**原因**：WSL 环境中使用了 Windows 安装的 Node.js [S005, S033]。

**解决方案**：

```bash
# 检查 Node.js 路径
which node
# 如果输出 /mnt/c/... 说明使用的是 Windows 版本

# 在 WSL 中安装 Linux 版本的 Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 20

# 确认路径正确
which node
# 应输出 /home/<user>/.nvm/versions/node/v20.x.x/bin/node
```

#### 问题：权限错误 (EACCES / Permission Denied)

**原因**：安装目录权限不足，或使用了 sudo 导致权限混乱 [S005, S032, S033]。

**解决方案**：

```bash
# 修复 npm 全局安装目录的权限（不要使用 sudo 安装）
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 重新安装（不要使用 sudo）
npm install -g @anthropic-ai/claude-code
```

### 13.2 配置类问题

#### 问题：API Key 无效

**解决方案** [S032, S035]：

排查步骤：
1. 确认 Key 格式正确（以 `sk-ant-` 开头）
2. 检查账户余额是否充足
3. 确认 Key 未过期或被撤销
4. 重新生成 Key 并完整复制（注意首尾空格）

```bash
# 重新设置 API Key
export ANTHROPIC_API_KEY="sk-ant-新生成的key"

# 或使用 OAuth 重新登录
claude /logout
claude  # 触发重新认证

# 测试连接
claude "你好"
```

#### 问题：登录状态丢失

**解决方案** [S032]：

```bash
# 退出并重新登录
claude /logout
claude

# 如果凭证存储出问题，检查目录权限
ls -la ~/.claude/
chmod 700 ~/.claude/
```

#### 问题：浏览器未自动打开（登录流程）

**解决方案** [S032]：

在某些终端环境中（如 SSH 远程连接、Docker 容器），浏览器无法自动打开。解决方案是手动复制终端输出的认证 URL 到本地浏览器完成登录。

### 13.3 运行时问题

#### 问题：高 CPU/内存占用

**原因**：通常由 MCP Server 或过大的上下文造成 [S005, S034]。

**解决方案**：

```bash
# 步骤 1：检查 MCP Server 状态
> /doctor

# 步骤 2：禁用不必要的 MCP Server
# 编辑 .claude/mcp.json，设置 disabled: true

# 步骤 3：压缩上下文
> /compact

# 步骤 4：如果问题持续，重启会话
> /clear

# 步骤 5：监控资源使用
top -pid $(pgrep -f "claude")
```

#### 问题：命令挂起/超时

**解决方案** [S005, S034]：

```bash
# 步骤 1：按 Ctrl+C 尝试中断

# 步骤 2：检查网络连接
curl -I https://api.anthropic.com

# 步骤 3：设置代理（如果需要）
export HTTPS_PROXY="http://proxy:port"

# 步骤 4：清除可能损坏的缓存
rm -rf ~/.claude/cache/

# 步骤 5：如有必要，完全退出并重启
```

#### 问题：搜索结果缓慢（WSL 环境）

**解决方案** [S005]：

```bash
# WSL 中文件系统操作比原生 Linux 慢
# 建议将项目放在 WSL 文件系统内，而非 /mnt/c/ 下

# 将项目迁移到 WSL 原生路径
cp -r /mnt/c/Projects/my-app ~/projects/my-app
cd ~/projects/my-app
claude
```

### 13.4 IDE 集成问题

#### 问题：JetBrains IDE 未检测到 Claude Code

**解决方案** [S005]：

```bash
# 在 WSL2 环境下，需要特殊网络配置
# 编辑 .wslconfig
[wsl2]
networkingMode=mirrored

# 在 JetBrains IDE 中配置终端
# Settings > Tools > Terminal > Shell Path
# 设置为: wsl.exe -d Ubuntu -e bash
```

#### 问题：VS Code 终端 Escape 键不工作

**解决方案** [S005]：

```json
// VS Code settings.json
{
  "terminal.integrated.commandsToSkipShell": [
    "-workbench.action.terminal.focusFind",
    "-workbench.action.quickOpen"
  ],
  "terminal.integrated.sendKeybindingsToShell": true
}
```

---

## 14. 调试技巧

### 14.1 执行追踪

Claude Code 的对话窗口本身就是完整的执行历史 [S036]。每个操作都会记录：

- **工具调用**：Read、Write、Edit、Bash 等工具的调用和结果
- **决策过程**：Claude 的分析和推理过程
- **子代理执行**：子代理的任务分解和执行状态

```
[Read] src/auth/jwt.ts
  --> 文件内容: ... (显示文件内容)

[Bash] npm test -- --filter auth
  --> 测试结果: 5 passed, 1 failed

[Edit] src/auth/jwt.ts
  --> 替换: line 42-45
  --> 变更预览: ...
```

**Subagent 追踪**：当 Claude 使用 Task Tool 创建子任务时，可以展开查看每个子任务的独立执行日志 [S036]。

### 14.2 日志分析

Claude Code 的日志存储在以下位置 [S034, S036]：

```bash
# macOS / Linux
~/.claude/logs/

# 查看最近的日志
ls -la ~/.claude/logs/
tail -f ~/.claude/logs/latest.log

# 搜索特定错误
grep -i "error" ~/.claude/logs/latest.log
```

**常见日志关键词解读** [S034, S036]：

| 错误信息 | 含义 | 解决方向 |
|---------|------|---------|
| `ECONNREFUSED` / `Authentication failed` | 连接被拒绝 / API 认证失败 | 网络问题或 API Key 无效 |
| `ETIMEOUT` / `Tool execution timeout` | 连接超时 / 工具执行超时 | 网络延迟或服务端响应慢 |
| `401 Unauthorized` | 认证失败 | API Key 无效或过期 |
| `429 Too Many Requests` / `Rate limit exceeded` | 请求频率限制 | 达到了 API 调用上限，等待后重试 |
| `context_length_exceeded` / `Context window exceeded` | 上下文溢出 | 使用 /compact 或 /clear |
| `MCP server disconnected` | MCP 服务断开 | 重启 MCP Server |

### 14.3 错误定位

**API 错误** [S033, S034]：

```bash
# 开启详细日志模式
export CLAUDE_DEBUG=1
claude

# 测试 API 连通性
curl -H "x-api-key: $ANTHROPIC_API_KEY" \
  https://api.anthropic.com/v1/messages \
  -d '{"model":"claude-sonnet-4-20250514","max_tokens":10,"messages":[{"role":"user","content":"test"}]}'
```

**系统错误** [S034]：

```bash
# 检查 Node.js 环境
node --version
npm --version

# 检查磁盘空间
df -h

# 检查内存使用
free -m  # Linux
vm_stat  # macOS
```

**配置错误** [S032, S033]：

```bash
# 验证 CLAUDE.md 语法
cat CLAUDE.md

# 验证 MCP 配置文件
cat .claude/mcp.json | python3 -m json.tool

# 验证 settings.json
cat .claude/settings.json | python3 -m json.tool
```

### 14.4 问题反馈

当遇到无法自行解决的问题时 [S001, S011]：

1. **GitHub Issues**：在 [Claude Code 官方仓库](https://github.com/anthropics/claude-code) 提交 Issue
2. **官方文档**：查阅 https://code.claude.com/docs 的 Troubleshooting 章节
3. **社区求助**：在 Claude 中文社区或论坛寻求帮助 [S012, S013]

**提交 Issue 时应包含的信息**：

```markdown
## 环境信息
- OS: macOS 14.5 / Ubuntu 22.04 / Windows 11 (WSL2)
- Node.js: v20.11.0
- Claude Code: v1.x.x
- Shell: zsh / bash

## 问题描述
[详细描述问题现象]

## 复现步骤
1. [步骤 1]
2. [步骤 2]
3. [步骤 3]

## 期望行为
[描述你期望的正常行为]

## 实际行为
[描述实际发生的问题]

## 日志输出
[粘贴相关的日志信息]
```

### 14.5 系统化排查流程（5 步法）

当遇到问题时，建议按以下流程系统化排查 [S033, S034, S032, S004, S036]：

```
步骤 1：检查系统环境
        +-- node --version (是否 >= 18)
        +-- npm --version
        +-- which claude (路径是否正确)
        +-- claude --version (版本是否最新)

步骤 2：验证 API 凭证
        +-- echo $ANTHROPIC_API_KEY (是否设置)
        +-- API Key 格式检查 (sk-ant-...)
        +-- 网络连通性测试 (curl api.anthropic.com)

步骤 3：查看配置文件
        +-- cat CLAUDE.md (项目级)
        +-- cat ~/.claude/CLAUDE.md (用户级)
        +-- cat .claude/mcp.json (MCP 配置)
        +-- cat ~/.claude/settings.json (全局设置)

步骤 4：重置配置（必要时）
        +-- mv ~/.claude ~/.claude.backup
        +-- claude (重新初始化)
        +-- 逐步恢复配置，定位问题项

步骤 5：查看日志输出
        +-- CLAUDE_DEBUG=1 claude (开启调试)
        +-- tail -f ~/.claude/logs/latest.log
        +-- 收集错误信息，提交反馈
```

**快速诊断命令汇总**：

```bash
# 一键诊断（在 Claude 会话中）
> /doctor

# 环境检查
node --version && npm --version && claude --version

# 网络检查
curl -I https://api.anthropic.com

# 配置检查
cat ~/.claude/CLAUDE.md 2>/dev/null && echo "---" && cat CLAUDE.md 2>/dev/null

# 日志检查
tail -50 ~/.claude/logs/latest.log 2>/dev/null
```

---

# 附录

## A. 实战案例精选

### 案例 1：1 小时搭建 Web 应用

以下是使用 Claude Code 快速搭建一个完整 Web 应用的流程 [S024]：

```bash
# 第1步：项目初始化（5分钟）
claude --dangerously-skip-permissions
> 创建一个 React + TypeScript + Vite 项目
> 添加 Tailwind CSS 和 shadcn/ui
> 配置 ESLint 和 Prettier

# 第2步：数据库设计（10分钟）
> 设计一个待办事项应用的数据库表结构
> 使用 Prisma 作为 ORM，SQLite 作为数据库，生成迁移文件

# 第3步：后端 API（15分钟）
> 实现 RESTful API：用户 CRUD 操作 + JWT 认证
> 添加错误处理
> 编写 API 测试

# 第4步：前端页面（20分钟）
> 实现登录页、注册页、待办事项列表页
> 使用 React Query 管理 API 状态
> 添加响应式布局

# 第5步：测试与部署（10分钟）
> 运行所有测试确认通过
> 添加 Dockerfile 和 docker-compose.yml
> 创建 GitHub Actions CI/CD 配置
```

### 案例 2：大型项目重构

处理复杂项目中的大规模重构任务 [S026]：

```bash
# 使用探索-规划-编码流程
claude

# 探索
> [Plan 模式] 这个 18000 行的 monolith 文件 src/app.ts 包含哪些主要功能模块？
> 请分析其中的函数调用关系和数据流

# 规划
> 制定将这个文件拆分为独立模块的计划，遵循 SOLID 原则
> 列出需要修改的文件清单和修改顺序

# 执行（分批进行）
> 第1步：提取 UserModule，保持所有测试通过
> 第2步：提取 PaymentModule，处理跨模块依赖
> ...继续提取其他模块...
```

### 案例 3：多 Claude 协作开发

模拟真实团队协作 [S020, S039]：

```bash
# 终端 1 (架构师 Claude / 测试优先)
claude --permission-mode plan
> 为电商项目设计微服务架构，定义服务边界和 API 接口
> 或：根据 PRD 文档为评论功能编写完整的测试套件

# 终端 2 (后端 Claude / 功能实现)
claude
> 根据 docs/architecture.md 中的设计，实现 OrderService
> 或：查看测试用例，实现评论功能使所有测试通过

# 终端 3 (前端 Claude)
claude
> 根据 docs/api-spec.md 中的接口定义，实现订单管理前端页面

# 终端 4 (测试/审查 Claude)
claude
> 为 OrderService 编写集成测试，覆盖所有 API 端点
> 或：持续监控代码变更，进行安全审查和代码质量检查
```

---

## B. 常用配置模板

### CLAUDE.md 完整模板

```markdown
# 项目：[项目名称]

## 概述
- 类型：[Web应用/CLI工具/库/...]
- 技术栈：[列出核心技术]
- 包管理器：[npm/yarn/pnpm]

## 架构
- [简要描述架构模式]
- 入口文件：[main entry point]
- 配置文件：[config locations]

## 编码规范
- 语言标准：[ES2022/TypeScript strict/...]
- 代码风格：[Prettier + ESLint]
- 命名约定：
  - 文件：[kebab-case/PascalCase/...]
  - 变量：[camelCase]
  - 类型：[PascalCase]
  - 常量：[UPPER_SNAKE_CASE]

## 测试
- 框架：[Jest/Vitest/...]
- 运行命令：[npm test]
- 覆盖率要求：[>80%]

## Git
- 分支策略：[Git Flow/Trunk-based]
- Commit 规范：[Conventional Commits]
- 常用分支：main, develop, feature/*, bugfix/*

## 常用命令
- 开发：[npm run dev]
- 构建：[npm run build]
- 测试：[npm test]
- 检查：[npm run lint]
- 部署：[npm run deploy]

## 注意事项
- [列出特殊的项目约束或注意点]

## 导入
@docs/architecture.md
@docs/api-guidelines.md
```

### settings.json 模板

```json
{
  "permissions": {
    "mode": "default",
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test*)",
      "Bash(npm run lint*)",
      "Bash(npm run build)",
      "Bash(git status)",
      "Bash(git diff*)",
      "Bash(git log*)"
    ],
    "deny": [
      "Bash(rm -rf*)",
      "Bash(sudo*)",
      "Bash(chmod 777*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
      }
    ],
    "Stop": [
      {
        "command": "osascript -e 'display notification \"Claude 完成\" with title \"Claude Code\"'"
      }
    ]
  }
}
```

### MCP Server 配置模板

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${BRAVE_API_KEY}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-playwright"]
    }
  }
}
```

---

## C. 资源链接

### 官方资源
- [Claude Code 官方文档](https://code.claude.com/docs) - 最权威的参考
- [Anthropic 官方博客](https://www.claude.com/blog) - 最新功能和技巧分享
- [Claude Code GitHub 仓库](https://github.com/anthropics/claude-code) - 源码和 Issue 追踪
- [Skills 完整指南 (PDF)](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) - 官方 Skills 开发手册
- [Skills 开发指南](https://platform.claude.com/docs/en/agents-and-tools/agent-skills) - 平台文档
- [MCP 协议规范](https://modelcontextprotocol.io) - MCP 官方规范

### 社区资源
- [Claude 中文社区](https://claudecn.com) - 中文教程和讨论
- [ClaudeCode101](https://claudecode101.com) - 系统化教程平台
- [Claude Code Hub](https://claudecodehub.github.io) - 社区文档聚合
- [Skills Fan](https://www.skills.fan) - Skills 资源站

### 学习资源
- [Claude Code 从入门到精通](https://www.cnblogs.com/leadingcode/p/19084161) - 综合中文教程
- [Claude Code 完全指南](https://www.cnblogs.com/knqiufan/p/19449849) - 全面详细的使用指南
- [菜鸟教程 Claude Code](http://www.runoob.com/ai-agent/claude-code.html) - 快速入门教程
- [视频教程](https://www.bilibili.com/video/BV1KzYQzWEPx/) - B 站实战教程

---

## D. 参考文献

### A 级来源（官方文档，8个）

| ID | 标题 | 来源 | 链接 |
|----|------|------|------|
| S001 | Claude Code 官方快速入门 | Anthropic 官方 | https://code.claude.com/docs/zh-CN/quickstart |
| S002 | Claude Code 概览 | Anthropic 官方 | https://code.claude.com/docs/zh-CN/overview |
| S003 | Claude Code 最佳实践 | Anthropic 官方 | https://code.claude.com/docs/en/best-practices |
| S004 | 内存管理官方文档 | Anthropic 官方 | https://code.claude.com/docs/zh-CN/memory |
| S005 | 官方故障排除文档 | Anthropic 官方 | https://code.claude.com/docs/zh-TW/troubleshooting |
| S006 | Skills 最佳实践 | Anthropic Platform | https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices |
| S007 | Skills 完整指南 PDF | Anthropic 资源 | https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf |
| S008 | Skills 和 MCP 扩展博客 | Anthropic 官方博客 | https://www.claude.com/blog/extending-claude-capabilities-with-skills-mcp-servers |

### B 级来源（权威社区，24个）

| ID | 标题 | 来源 | 链接 |
|----|------|------|------|
| S009 | Claude Code 从入门到精通 | 博客园 | https://www.cnblogs.com/leadingcode/p/19084161 |
| S010 | 最全 Claude Code 使用指南 | 知乎 | https://zhuanlan.zhihu.com/p/1954938233126381432 |
| S011 | Claude Code 完整使用指南 | GitHub | https://github.com/Joseph19820124/claude-code-guide |
| S012 | ClaudeCode 教程中心 | 社区教程 | https://claudecode101.com/zh/tutorial |
| S013 | Claude 中文入门指南 | Claude 中文社区 | https://claudecn.com/docs/claude-code/getting-started/ |
| S014 | Skills 和 MCP Servers 能力 | Medium | https://medium.com/@moelkholy1995/claude-code-capabilities-with-skills-and-mcp-servers-2b5e87ea8e0d |
| S015 | MCP Server 完整配置指南 | BrainGrid | https://www.braingrid.ai/blog/claude-code-mcp |
| S016 | MCP 上下文优化 | Scott Spence | https://scottspence.com/posts/optimising-mcp-server-context-usage-in-claude-code |
| S017 | Skills vs MCP vs Agents | Colin McNamara | https://colinmcnamara.com/blog/understanding-skills-agents-and-mcp-in-claude-code |
| S018 | MCP 工具配置指南 | Clockwise | https://www.getclockwise.com/blog/claude-code-mcp-tools-integration |
| S019 | Claude Code 中高级技巧 | 天盘博客 | https://tianpan.co/zh/blog/2025-08-21-claude-code-tips |
| S020 | 多 Claude 协作工作流 | Claude 中文 | https://claudecn.com/docs/claude-code/workflows/multi-claude/ |
| S021 | Skills 实用指南 | 腾讯新闻 | https://news.qq.com/rain/a/20260116A01DJS00 |
| S023 | AI编程实践与协作 | 得物技术/腾讯云 | https://cloud.tencent.com/developer/article/2624381 |
| S026 | Claude Code 完全指南 | 博客园 | https://www.cnblogs.com/knqiufan/p/19449849 |
| S027 | CLAUDE.md 8条黄金法则 | DAMO开发者 | https://damodev.csdn.net/696c8c73a16c6648a98333c7.html |
| S028 | 从0到1搭建项目环境 | 腾讯云 | https://cloud.tencent.com/developer/article/2596824 |
| S029 | Mintlify 文档配置 | Mintlify | https://www.mintlify.com/docs/zh/guides/claude-code |
| S030 | Claude 中文网内存管理 | Claude 中文网 | https://www.claude-cn.org/claude-code-docs-zh/configuration/memory |
| S031 | Claude Code Hub 内存 | Community Hub | https://claudecodehub.github.io/memory.html |
| S032 | Skills Fan FAQ | Skills Fan | https://www.skills.fan/docs/troubleshooting/faq |
| S033 | 完整故障排查指南 | Hrefgo | https://hrefgo.com/zh/blog/claude-code-faq-troubleshooting-support |
| S034 | 架构层面故障排除 | 腾讯云社区 | https://cloud.tencent.com/developer/article/2555179 |
| S035 | Claude Code Pro FAQ | ClaudeCode.pro | https://claudecode.pro/zh/docs/faq |
| S039 | 多Claude协作教程 | ClaudeCode101 | https://claudecode101.com/zh/tutorial/advanced/multi-claude |

### C 级来源（经验分享，8个）

| ID | 标题 | 来源 | 链接 |
|----|------|------|------|
| S022 | 高阶用法与骚技巧 | DEV Community | https://dev.to/dragon72463399/claude-code-gao-jie-yong-fa-sao-ji-qiao-2bf7 |
| S024 | 1小时搞定Web应用 | 腾讯云社区 | https://cloud.tencent.com/developer/news/2853975 |
| S025 | B站实战教程 | Bilibili | https://www.bilibili.com/video/BV1KzYQzWEPx/ |
| S036 | 傻瓜式调试技巧 | Medium | https://medium.com/@marklaik/day-12-傻瓜式調適-claude-code產生的bugs-b6f4777c452e |
| S037 | 高级技巧效率翻倍 | 53AI | https://www.53ai.com/news/tishicijiqiao/2025092397623.html |
| S038 | 李锋镝高级技巧 | 个人博客 | https://www.lifengdi.com/ren-gong-zhi-neng/4619 |
| S040 | 菜鸟教程入门 | 菜鸟教程 | http://www.runoob.com/ai-agent/claude-code.html |

---

> **免责声明**：本报告基于 2026 年 2 月的信息编写。Claude Code 作为一个快速迭代的产品，其功能和配置可能随版本更新而变化。建议读者结合 [官方文档](https://code.claude.com/docs) 获取最新信息。
>
> **报告版本**：V1.0 (Final) | **最后更新**：2026-02-10
