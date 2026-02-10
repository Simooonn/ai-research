# Claude Code 使用方式与高效技巧：从入门到精通

> **版本**: V1.0 | **日期**: 2026-02-10 | **字数**: 约14,000字
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

简而言之，Claude Code 就像一位驻扎在你终端里的高级工程师，能够阅读你的整个代码库，理解项目上下文，并执行从代码编写到测试、部署的完整开发流程 [S009]。

### 1.2 核心特性与优势

Claude Code 的核心竞争力体现在以下四个维度：

**超大上下文窗口 (200K tokens)**：Claude Code 支持高达 200K token 的上下文窗口 [S002, S026]，这意味着它可以同时"看到"大量的代码文件、文档和对话历史。在实际开发中，这使得 Claude Code 能够理解复杂项目的完整架构，而不是仅限于单个文件的片段。

**系统级权限访问**：不同于受限的沙盒环境，Claude Code 拥有完整的系统级权限 [S026]。它可以读写文件、执行 shell 命令、访问网络资源。这种权限模型使其能够完成真正的端到端开发任务，而非仅提供代码建议。

**多种扩展机制 (Skills/MCP/Hooks)**：Claude Code 通过三大扩展体系实现能力增强 [S008, S026]——Skills（技能）为其注入领域知识和工作流程，MCP Server（模型上下文协议服务器）让它连接外部工具和数据源，Hooks（钩子）允许在关键事件点执行自定义脚本。

**多代理协作能力**：Claude Code 支持多个实例并行工作，实现编码、审查、测试的流水线式协作 [S020, S026]。这在处理大型项目或紧急任务时尤为有效。

### 1.3 适用场景

Claude Code 最擅长的开发场景包括 [S019]：

| 场景 | 说明 | 典型命令 |
|------|------|----------|
| **代码理解与问答** | 快速理解陌生代码库的架构和逻辑 | `解释这个项目的目录结构` |
| **大规模重构** | 跨文件的代码重组和模式迁移 | `将所有 class 组件迁移到 hooks` |
| **调试与错误修复** | 定位和修复复杂 bug | `分析这个错误日志并修复` |
| **复杂功能生成** | 生成完整的功能模块（含测试） | `创建一个用户认证模块` |
| **测试驱动开发** | 编写测试用例并实现通过代码 | `为 UserService 编写单元测试` |
| **Git 操作自动化** | 自动生成 commit 信息、创建 PR | `/commit` |

---

## 2. 安装与配置

### 2.1 系统要求

在安装 Claude Code 之前，请确保系统满足以下要求 [S001, S033]：

| 要求 | 最低版本 | 推荐版本 | 备注 |
|------|----------|----------|------|
| **Node.js** | 18.0+ | 20 LTS | 必须依赖 |
| **操作系统** | macOS 12+ / Ubuntu 20.04+ / Windows (WSL2) | macOS 14+ / Ubuntu 22.04+ | Windows 需通过 WSL |
| **内存** | 4 GB | 8 GB+ | MCP Server 较多时需更多内存 |
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

Claude Code 的基本使用方式从启动一个交互会话开始 [S001]：

```bash
# 在项目根目录启动（推荐）
cd /path/to/your/project
claude

# 直接给出任务（非交互模式）
claude "解释这个项目的架构"

# 恢复上一次会话
claude --resume

# 从管道输入
cat error.log | claude "分析这个错误日志"
```

**最佳实践**：始终在项目根目录启动 Claude Code，这样它能自动发现 `CLAUDE.md` 配置文件并理解项目结构 [S001, S010]。

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

使用示例：

```bash
# 启动后查看帮助
claude
> /help

# 管理项目记忆
> /memory

# 压缩对话以节省上下文空间
> /compact

# 一键提交代码
> /commit
```

### 3.3 文件操作

Claude Code 提供四种核心文件操作工具 [S003, S036]：

**Read（读取文件）**：读取文件内容并加载到上下文中。

```
> 阅读 src/index.ts 文件并解释其功能
```

Claude Code 会使用 Read 工具获取文件内容，然后进行分析。

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

Claude Code 会使用 `**/*.test.ts` 等模式进行搜索。

**文件操作的最佳实践** [S003]：
- 优先使用 Edit 而非 Write，以减少意外覆盖
- 使用 Glob 精确定位文件，避免不必要的全目录扫描
- 对关键文件修改前，先用 Read 确认当前状态

### 3.4 权限模式

Claude Code 提供四种权限模式，控制其自主操作的级别 [S019, S024]：

| 模式 | 级别 | 说明 | 适用场景 |
|------|------|------|----------|
| `default` | 安全 | 每次操作都需要确认 | 生产环境、敏感操作 |
| `acceptEdits` | 中等 | 自动接受文件编辑，其他需确认 | 日常开发 |
| `plan` | 只读 | 只规划不执行 | 方案评审、架构设计 |
| `dangerously-skip-permissions` | 无限制 | 跳过所有权限确认 | 个人实验、一次性脚本 |

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

默认模式是 Claude Code 的标准工作方式 [S019]。在此模式下：

- Claude 会在执行任何写操作之前请求确认
- 读取文件和分析代码不需要确认
- 适合处理重要项目和生产环境代码

```bash
# 默认模式启动
claude

# 在默认模式下的交互示例
> 重构 UserService 中的认证逻辑
# Claude 会先分析代码，然后展示修改方案
# 每个文件的修改都会显示 diff 并等待你确认
```

### 4.2 Auto 自动模式（YOLO 模式）

Auto 模式让 Claude Code 获得更大的自主权，可以自动执行大多数操作而无需逐一确认 [S024]。这种模式特别适合：

- 快速原型开发
- 一次性脚本编写
- 个人实验项目

```bash
# 启动 Auto 模式
claude --dangerously-skip-permissions

# 或者在会话中通过 Shift+Tab 切换
```

**Auto 模式的典型工作流** [S024]：

```
> 创建一个完整的 Express.js REST API，包含用户 CRUD 操作、JWT 认证、错误处理和单元测试
```

在 Auto 模式下，Claude 会自动：
1. 初始化项目结构
2. 安装依赖包
3. 创建所有源文件
4. 编写测试代码
5. 运行测试确认通过

### 4.3 Plan 计划模式

Plan 模式让 Claude 只进行分析和规划，不执行任何实际修改 [S019, S027]。这在以下场景特别有用：

- 评估重构方案的可行性
- 理解复杂代码变更的影响范围
- 在正式开发前审查 Claude 的思路

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

**推荐的模式组合策略**：

```
第一步：Plan 模式 → 制定方案
        ↓ (确认方案合理)
第二步：Default 模式 → 逐步执行关键修改
        ↓ (确认方向正确)
第三步：Auto 模式 → 批量完成剩余工作
```

---

## 5. 上下文管理

### 5.1 CLAUDE.md 核心概念

`CLAUDE.md` 是 Claude Code 最重要的配置机制，它相当于**项目的记忆文件**——告诉 Claude 关于你的项目的一切重要信息 [S004, S019, S027]。每次会话开始时，Claude Code 会自动读取并加载 `CLAUDE.md` 的内容作为系统上下文。

### 5.2 三层内存架构

Claude Code 采用分层的内存体系，从高到低分为三层 [S004, S030]：

```
┌─────────────────────────────────────┐
│  企业策略层 (Enterprise Policy)      │  ← 组织统一标准
│  ~/.config/claude-code/CLAUDE.md    │
├─────────────────────────────────────┤
│  项目内存层 (Project Memory)         │  ← 团队共享规范
│  <project-root>/CLAUDE.md           │
├─────────────────────────────────────┤
│  用户内存层 (User Memory)            │  ← 个人偏好
│  ~/.claude/CLAUDE.md                │
└─────────────────────────────────────┘
```

**企业策略层**：位于 `~/.config/claude-code/CLAUDE.md`，适合设置组织级别的统一规范，例如安全策略、代码风格标准等 [S004]。

**项目内存层**：位于项目根目录的 `CLAUDE.md`，这是最常用的配置层。包含项目特定的信息，如技术栈、架构说明、编码规范等 [S004, S027]。此文件通常纳入 Git 版本控制，团队成员共享。

**用户内存层**：位于 `~/.claude/CLAUDE.md`，存储个人偏好设置，例如输出语言、编辑器风格、常用快捷方式等 [S004, S030]。

### 5.3 CLAUDE.md 编写最佳实践：8 条黄金法则

根据社区和官方推荐，编写 `CLAUDE.md` 应遵循以下 8 条黄金法则 [S027]：

**法则 1：保持简短精炼**
`CLAUDE.md` 的内容会占用上下文窗口，过长的配置会挤压实际工作的可用空间。

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
使用 Markdown 标题和列表来组织信息，便于 Claude 快速定位关键内容。

**法则 3：避免冗余信息**
不要重复 Claude 已经能从代码中推断的信息（如 `package.json` 中已有的依赖列表）。

**法则 4：具体而非抽象**
提供具体的规范而非模糊的描述。

```markdown
<!-- 好的写法 -->
- 函数命名使用 camelCase
- 组件文件使用 PascalCase.tsx
- API 路由文件使用 kebab-case.ts

<!-- 避免的写法 -->
- 请遵循良好的命名规范
```

**法则 5：包含关键上下文**
记录那些 Claude 无法从代码中自动推断的信息，如架构决策背景、特殊的业务逻辑约束等。

**法则 6：定期更新维护**
项目演进时同步更新 `CLAUDE.md`，保持其与实际代码的一致性。

**法则 7：使用导入机制**
将大型配置拆分为多个文件，使用 `@import` 语法引用。

**法则 8：分离公私配置**
项目共享配置放在项目根目录的 `CLAUDE.md`，个人偏好放在 `~/.claude/CLAUDE.md`。

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
    ├── coding-style.md       # 编码风格规则
    ├── testing.md             # 测试相关规则
    ├── security.md            # 安全规则
    └── api-design.md          # API 设计规则
```

规则文件支持 **Glob 模式匹配**，可以为特定类型的文件定义专属规则 [S004]：

```markdown
<!-- .claude/rules/react-components.md -->
<!-- 仅对 src/components/**/*.tsx 生效 -->

# React 组件规则
- 所有组件必须导出 Props 类型定义
- 使用 forwardRef 处理 ref 传递
- 必须包含 displayName
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

Skills（技能）是 Claude Code 中用于注入**领域知识和工作流程**的扩展机制 [S006, S007, S008]。Skills 本质上是一组结构化的 Markdown 指令文件，告诉 Claude 在特定场景下应该如何思考和行动。

**Skills vs MCP vs Agents 的区别** [S017]：

| 维度 | Skills | MCP Server | Agents/Subagents |
|------|--------|------------|------------------|
| **本质** | 知识和流程指令 | 外部工具连接 | 并行任务执行 |
| **作用** | 教 Claude "怎么做" | 让 Claude "能做什么" | 让 Claude "同时做" |
| **形式** | Markdown 文件 | JSON 配置 + 服务进程 | 多个 Claude 实例 |
| **可移植性** | 高（纯文本） | 中（需服务环境） | 低（需多终端） |
| **Token 消耗** | 低 | 较高 | 分散 |

**工作原理**：当你通过斜线命令调用一个 Skill 时，Claude Code 会加载对应的 `SKILL.md` 文件到上下文中，从而获得该领域的专业知识和操作流程 [S007, S008]。

### 6.2 官方 Skills 推荐

以下是社区和官方推荐的高质量 Skills [S021]：

**Superpowers（超能力）**：GitHub 上获得超过 1.6 万 Star 的综合增强 Skill，为 Claude Code 注入高级编程和分析能力 [S021]。

```bash
# 安装 Superpowers Skill
claude install-skill https://github.com/anthropics/superpowers-skill
```

**Planning-with-files（文件规划）**：帮助 Claude 在复杂项目中进行系统化的文件级规划 [S021]。

**UI UX Pro Max**：专注于 UI/UX 设计和前端开发的 Skill，包含设计系统知识和组件最佳实践 [S021]。

**seo-review / content-creator**：SEO 审查和内容创作的专业 Skill [S021]。

**skill-prompt-generator**：帮助你创建自定义 Skill 的元 Skill [S021]。

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
    │   └── templates/
    └── examples/
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

### 步骤 4：代码质量
- [ ] 命名是否清晰
- [ ] 函数是否过长（>50行警告）
- [ ] 错误处理是否完整
- [ ] 类型定义是否准确

## 输出格式
以 Markdown 表格输出审查结果：
| 严重级别 | 位置 | 问题描述 | 建议修改 |

## 约束
- 不要自动修改代码，只提供建议
- 始终解释问题的原因，而不仅仅指出问题
```

**Skills 开发最佳实践** [S006, S007]：

1. **单一职责原则**：每个 Skill 聚焦一个特定任务
2. **步骤分解**：将复杂任务拆分为清晰的步骤序列
3. **明确的输出格式**：定义结构化的输出模板
4. **包含约束条件**：设定 Skill 的行为边界
5. **提供示例**：在 `examples/` 目录下放置参考示例

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
- Type validation（类型验证）

### 3. 测试规范
- 使用 describe/it 结构
- 每个 it 块只测试一个行为
- 使用 AAA 模式 (Arrange-Act-Assert)
- Mock 外部依赖

### 4. 验证
- 运行生成的测试确保通过
- 检查覆盖率
```

---

## 7. MCP Server 集成

### 7.1 MCP 协议介绍

MCP（Model Context Protocol，模型上下文协议）是 Anthropic 提出的一种标准化协议，用于连接 AI 模型与外部工具和数据源 [S008, S015]。它被比喻为 **"AI 的 USB-C"**——一个通用的接口标准 [S018]。

通过 MCP Server，Claude Code 可以：
- 搜索互联网获取最新信息
- 直接操作数据库
- 管理 GitHub 仓库
- 访问 Figma 设计文件
- 执行浏览器自动化测试

### 7.2 配置方法

MCP Server 通过 JSON 配置文件进行管理 [S015, S018]。

**配置文件位置**：

```bash
# 项目级配置（推荐）
<project-root>/.claude/mcp.json

# 用户级配置
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
        "API_KEY": "your-api-key"
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

### 7.3 常用 MCP Servers

以下是最常用和最实用的 MCP Server 配置示例 [S015, S018]：

#### Brave Search - 网页搜索

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-brave-api-key"
      }
    }
  }
}
```

使用场景：让 Claude Code 搜索最新的技术文档、bug 修复方案等。

#### GitHub MCP - 代码仓库操作

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

使用场景：管理 Issue、查看 PR、搜索代码仓库。

#### Playwright - 浏览器自动化

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-playwright"]
    }
  }
}
```

使用场景：自动化测试、网页截图、表单填写。

#### Supabase - 数据库集成

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-supabase"],
      "env": {
        "SUPABASE_URL": "https://your-project.supabase.co",
        "SUPABASE_KEY": "your-service-key"
      }
    }
  }
}
```

使用场景：直接查询和修改数据库，管理表结构。

#### Figma - 设计文件访问

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-figma"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "your-figma-token"
      }
    }
  }
}
```

使用场景：读取 Figma 设计稿，提取设计 token，辅助前端还原。

### 7.4 上下文优化（Token 消耗问题）

MCP Server 是一把双刃剑。每个启用的 MCP Server 都会将其工具描述加载到上下文中，**即使你没有使用它**。根据实测数据，单个 MCP Server 可能消耗 **数千到数万 tokens** 的上下文空间 [S016]。

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

2. **使用 `/doctor` 命令**监控上下文使用情况 [S016]

```bash
> /doctor
# 查看当前 MCP Server 的 Token 消耗情况
```

3. **项目级配置**：为不同项目配置不同的 MCP Server 集合，避免全局加载过多

4. **定期清理**：移除不再使用的 MCP Server 配置

### 7.5 MCP 实战案例

**案例：使用 GitHub MCP 自动化 PR 审查**

```bash
# 确保 GitHub MCP 已配置
claude

> 请审查 PR #42 的所有变更，重点检查安全问题和性能影响，
> 然后在 PR 上添加审查评论
```

Claude Code 会通过 GitHub MCP Server：
1. 获取 PR #42 的所有变更文件
2. 分析每个变更的影响
3. 自动在 GitHub 上提交 Review 评论

---

## 8. Hooks 钩子系统

### 8.1 工作原理

Hooks（钩子）是 Claude Code 的事件驱动扩展机制，允许在 Claude 执行特定操作的前后自动运行自定义脚本 [S009, S022]。

**支持的 Hook 事件类型** [S009]：

| 事件 | 触发时机 | 典型用途 |
|------|----------|----------|
| `PreToolUse` | 工具调用之前 | 参数验证、权限检查 |
| `PostToolUse` | 工具调用之后 | 结果处理、日志记录 |
| `Notification` | Claude 发送通知时 | 消息推送、状态更新 |
| `Stop` | Claude 完成任务时 | 最终检查、清理工作 |

### 8.2 常用钩子场景

#### 场景一：代码格式化（保存后自动格式化）

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
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
        "command": "bash -c 'if [[ \"$CLAUDE_FILE_PATH\" == *.test.* ]]; then npx jest \"$CLAUDE_FILE_PATH\" --no-coverage; fi'"
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
        "matcher": "Bash",
        "command": "bash -c 'if echo \"$CLAUDE_TOOL_INPUT\" | grep -q \"git commit\"; then npx lint-staged; fi'"
      }
    ]
  }
}
```

在 Claude 执行 `git commit` 之前，自动运行 lint-staged 检查 [S009]。

#### 场景四：通知推送

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "osascript -e 'display notification \"Claude 任务完成\" with title \"Claude Code\"'"
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

### 8.4 Hooks 最佳实践

1. **保持轻量级**：Hook 脚本应快速执行，避免耗时操作阻塞 Claude 的主流程 [S009]
2. **错误不阻塞**：Hook 脚本的失败不应中断 Claude 的工作 [S009]
3. **记录日志**：为复杂的 Hook 添加日志，便于调试 [S009]
4. **使用 matcher 精确匹配**：通过正则表达式精确控制 Hook 的触发范围

---

## 9. Subagents 子代理

### 9.1 协作模式

Subagents（子代理）是 Claude Code 的多实例并行协作机制 [S017, S020, S023]。当面临复杂的、可并行化的任务时，Claude Code 可以启动多个子代理同时工作。

与 Skills 和 MCP 的本质区别在于：Skills 注入知识，MCP 连接工具，而 **Subagents 提供并行执行能力** [S017]。

### 9.2 并行任务处理（Task Tool）

Claude Code 内置的 **Task Tool** 允许将任务分派给子代理 [S020, S036]：

```
> 请同时完成以下任务：
> 1. 为 UserService 编写单元测试
> 2. 为 AuthService 编写单元测试
> 3. 为 PaymentService 编写单元测试
```

在此场景下，Claude Code 会启动多个子代理并行处理这三个独立的测试编写任务，每个子代理拥有独立的上下文空间 [S020, S023]。

**任务分解策略** [S023]：

- **按功能模块分解**：不同模块分配给不同子代理
- **按阶段分解**：分析阶段和实现阶段分别处理
- **按文件类型分解**：前端和后端代码分开处理

**上下文隔离**：每个子代理拥有独立的上下文窗口，它们之间不共享对话历史 [S020, S023]。这意味着需要在任务描述中提供足够的背景信息。

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

#### 工作流三：并行特性开发

```bash
# 终端 1：Claude 开发 Feature A
claude
> 在 feature/user-profile 分支上实现用户资料编辑功能

# 终端 2：Claude 开发 Feature B
claude
> 在 feature/notification 分支上实现通知系统

# 注意：确保两个 Claude 工作在不同的 Git 分支上，避免文件冲突
```

**并行开发注意事项** [S020, S023]：
- 确保不同 Claude 实例操作不同的文件或分支，避免冲突
- 明确每个实例的责任边界
- 定期同步状态（例如通过 Git merge）

### 9.4 企业实践：得物技术的经验

得物技术团队在大规模 AI 编程实践中，总结了以下 Subagent 协作经验 [S023]：

1. **对话流设计**：将复杂业务需求拆解为多轮对话流，每轮明确输入和期望输出
2. **子代理系统分层**：按领域划分子代理职责（如前端 Agent、后端 Agent、测试 Agent）
3. **质量门禁**：在子代理输出和主流程之间设置检查点，确保每个阶段的产出质量
4. **经验沉淀**：将成功的协作模式固化为 Skills 或 CLAUDE.md 配置，实现团队复用

---

# 第四部分：工作流优化

## 10. 高效工作流

### 10.1 探索-规划-编码流程

Anthropic 官方推荐的三阶段开发流程 [S003, S012, S019]：

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Explore  │ ──→ │   Plan   │ ──→ │   Code   │
│  探索阶段  │     │  规划阶段  │     │  编码阶段  │
└──────────┘     └──────────┘     └──────────┘
```

**探索阶段 (Explore)**：

```
> 请分析这个项目的整体架构，重点关注：
> 1. 目录结构和模块划分
> 2. 核心数据流
> 3. 外部依赖和集成点
> 4. 现有的测试覆盖情况
```

在这个阶段，让 Claude 充分理解代码库，识别关键文件和依赖关系 [S003, S019]。

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

确认规划方案后，切换到 Default 或 Auto 模式开始执行 [S003]。

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

### 10.3 Git 集成最佳实践

Claude Code 提供深度的 Git 集成 [S009]：

#### 自动生成 commit 信息

```bash
# 使用 /commit 命令
> /commit

# Claude 会自动：
# 1. 分析 git diff
# 2. 生成符合 Conventional Commits 规范的信息
# 3. 等待你确认后执行 commit

# 或者直接描述意图
> 提交当前的修改，commit 信息要说明我们修复了登录超时的问题
```

#### 一键创建 PR

```bash
# 使用 /pr 命令
> /pr

# Claude 会自动：
# 1. 推送当前分支到远程
# 2. 分析所有 commit 的变更
# 3. 生成 PR 标题和描述
# 4. 通过 gh CLI 创建 PR
```

#### 根据 Issue 修复

```bash
# 直接告诉 Claude Issue 号
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
          claude --headless "审查这个 PR 的所有变更，检查安全性和代码质量问题" \
            --output-format json > review-results.json

      - name: Post Review Comments
        uses: actions/github-script@v7
        with:
          script: |
            const results = require('./review-results.json');
            // 将审查结果作为 PR 评论发布
```

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
| **Tab** | 自动补全 | 命令和路径补全 |
| **Ctrl+C** | 中断当前操作 | 停止正在执行的任务 |
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
  "terminal.integrated.enableImages": true
}
```

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
```

### 11.4 批量操作技巧

**多文件批量修改** [S019]：

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

---

## 12. 性能优化

### 12.1 上下文窗口管理

Claude Code 的 200K token 上下文窗口虽然很大，但在复杂项目中仍然需要谨慎管理 [S002, S026]：

**监控上下文使用**：

```bash
# 查看当前上下文使用情况
> /cost

# 使用 /doctor 进行系统诊断
> /doctor
```

**清理无用上下文** [S003]：

```bash
# 压缩对话历史（保留关键信息，删除冗余部分）
> /compact

# 完全重置上下文
> /clear
```

**上下文管理原则**：

- 单个会话专注于一个任务，避免在一个长会话中处理多个不相关任务
- 及时使用 `/compact` 压缩不再需要的对话历史
- 大型文件不要完整加载，指定需要的行范围

### 12.2 Token 使用优化

减少 Token 消耗不仅能降低成本，还能提高响应速度 [S003, S016]：

**减少不必要的文件读取**：

```
# 好的做法：指定需要查看的范围
> 阅读 src/services/auth.ts 中的 login 函数

# 避免的做法：加载整个大文件
> 阅读 src/services/auth.ts 的全部内容
```

**使用 Glob 精确匹配** [S003]：

```
# 好的做法：精确匹配
> 找到所有以 .service.ts 结尾的文件

# 避免的做法：模糊搜索
> 搜索项目中所有文件
```

**避免重复加载** [S016]：
- Claude 会缓存已读取的文件内容
- 如果文件没有变化，不需要重复要求 Claude 阅读

### 12.3 缓存策略

**文件缓存**：Claude Code 在会话期间会缓存已读取的文件内容。利用这一点 [S037]：

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

**网络优化** [S033, S034]：
- 使用稳定且低延迟的网络连接
- 如果使用代理，确保代理服务稳定
- 设置合理的超时时间

**减少 MCP Server 开销** [S016]：
- 只启用当前任务需要的 MCP Server
- 使用 `disabled` 字段暂时关闭不需要的 Server

**并发请求控制** [S037]：
- 避免在高峰时段提交大量请求
- 对于批量任务，考虑分批处理

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

#### 问题：权限错误 (EACCES)

**原因**：安装目录权限不足，或使用了 sudo 导致权限混乱 [S005, S032, S033]。

**解决方案**：

```bash
# 修复 npm 全局安装目录的权限
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

```bash
# 步骤 1：确认 API Key 格式正确（以 sk-ant- 开头）
echo $ANTHROPIC_API_KEY

# 步骤 2：检查账户余额
# 访问 https://console.anthropic.com/billing

# 步骤 3：重新生成 Key
# 访问 https://console.anthropic.com/keys

# 步骤 4：更新环境变量
export ANTHROPIC_API_KEY="sk-ant-新生成的key"

# 步骤 5：测试连接
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

```bash
# Claude 会在终端显示登录 URL
# 手动复制链接到浏览器完成认证
# 示例输出：
# Please visit: https://claude.ai/oauth/authorize?...
# 复制此链接到浏览器打开
```

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
```

#### 问题：命令挂起

**解决方案** [S005, S034]：

```bash
# 步骤 1：按 Ctrl+C 尝试中断
# 步骤 2：检查网络连接
ping api.anthropic.com

# 步骤 3：设置超时
export ANTHROPIC_TIMEOUT=30000

# 步骤 4：如有必要，完全退出并重启
# 按 Ctrl+C 多次退出，然后重新启动 claude
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
# 在 WSL2 环境下，检查网络模式
# 打开 Windows Terminal 运行:
wsl --version
# 确保 WSL 版本 >= 2.0

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
    "-workbench.action.terminal.focusFind"
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

**查看详细执行信息**：

```bash
# 展开工具调用的详细信息（在终端中点击折叠区域）
# 每个工具调用会显示：
# - 工具名称
# - 输入参数
# - 执行结果
# - 消耗的 Token 数

# 使用 Subagent 追踪
# 当 Claude 使用 Task Tool 时，可以看到：
# - 子任务分解
# - 每个子代理的执行状态
# - 结果汇总
```

### 14.2 日志分析

Claude Code 的日志存储在以下位置 [S034, S036]：

```bash
# macOS / Linux
~/.claude/logs/

# 查看最近的日志
ls -la ~/.claude/logs/
cat ~/.claude/logs/latest.log

# 搜索特定错误
grep -i "error" ~/.claude/logs/latest.log
```

**常见日志关键词解读** [S034, S036]：

| 关键词 | 含义 | 通常原因 |
|--------|------|----------|
| `ECONNREFUSED` | 连接被拒绝 | 网络问题或 API 服务不可用 |
| `ETIMEOUT` | 连接超时 | 网络延迟或服务端响应慢 |
| `401 Unauthorized` | 认证失败 | API Key 无效或过期 |
| `429 Too Many Requests` | 请求频率限制 | 达到了 API 调用上限 |
| `context_length_exceeded` | 上下文溢出 | 对话或文件内容超出限制 |

### 14.3 错误定位

**API 错误** [S033, S034]：

```bash
# 测试 API 连通性
curl -H "Authorization: Bearer $ANTHROPIC_API_KEY" \
  https://api.anthropic.com/v1/models

# 检查 API Key 是否有效
echo $ANTHROPIC_API_KEY | head -c 10
# 应输出 sk-ant-xxx
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
cat .claude/mcp.json | python -m json.tool

# 验证 settings.json
cat .claude/settings.json | python -m json.tool
```

### 14.4 问题反馈

当遇到无法自行解决的问题时 [S001, S011]：

1. **GitHub Issues**：在 [Claude Code 官方仓库](https://github.com/anthropics/claude-code) 提交 Issue
2. **官方支持**：通过 Anthropic 官方支持渠道反馈
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
        ├── Node.js 版本 >= 18
        ├── npm 版本是否匹配
        └── 操作系统是否在支持列表中

步骤 2：验证 API 凭证
        ├── API Key 格式正确 (sk-ant-)
        ├── 账户余额充足
        └── 网络可以访问 api.anthropic.com

步骤 3：查看配置文件
        ├── CLAUDE.md 语法正确
        ├── mcp.json 格式有效
        └── settings.json 配置正确

步骤 4：重置配置（必要时）
        ├── /clear 重置上下文
        ├── 删除 ~/.claude/cache/
        └── 重新安装 Claude Code

步骤 5：查看日志输出
        ├── 检查 ~/.claude/logs/
        ├── 搜索错误关键词
        └── 收集信息提交 Issue
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
# 步骤 1：初始化项目
claude --dangerously-skip-permissions
> 创建一个待办事项 Web 应用，使用 Next.js + TypeScript + Prisma + SQLite

# 步骤 2：Claude 会自动
# - 初始化 Next.js 项目
# - 配置 TypeScript
# - 设置 Prisma 和数据库
# - 创建 CRUD API
# - 构建前端 UI
# - 添加样式

# 步骤 3：测试和优化
> 为所有 API 端点添加单元测试，并修复发现的问题

# 步骤 4：部署准备
> 添加 Dockerfile 和 docker-compose.yml，配置生产环境部署
```

### 案例 2：大型项目重构

处理复杂项目中的大规模重构任务 [S026]：

```bash
# 使用探索-规划-编码流程
claude

# 探索
> 这个 18000 行的 monolith 文件 src/app.ts 包含哪些主要功能模块？
> 请分析其中的函数调用关系和数据流

# 规划
> 制定将这个文件拆分为独立模块的计划，遵循 SOLID 原则

# 执行（分批进行）
> 先提取 UserService 模块到独立文件，确保所有测试通过
> ...继续提取其他模块...
```

### 案例 3：多 Claude 协作开发

模拟真实团队协作 [S020, S039]：

```bash
# 终端 1 (架构师 Claude)
claude --permission-mode plan
> 为电商项目设计微服务架构，定义服务边界和 API 接口

# 终端 2 (后端 Claude)
claude
> 根据 docs/architecture.md 中的设计，实现 OrderService

# 终端 3 (前端 Claude)
claude
> 根据 docs/api-spec.md 中的接口定义，实现订单管理前端页面

# 终端 4 (测试 Claude)
claude
> 为 OrderService 编写集成测试，覆盖所有 API 端点
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
    "mode": "acceptEdits",
    "allowedCommands": [
      "npm test",
      "npm run lint",
      "npm run build",
      "git status",
      "git diff",
      "git log"
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
        "BRAVE_API_KEY": "your-brave-api-key"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
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

### 社区资源
- [Claude 中文社区](https://claudecn.com) - 中文教程和讨论
- [ClaudeCode101](https://claudecode101.com) - 系统化教程平台
- [Claude Code Hub](https://claudecodehub.github.io) - 社区文档聚合

### 学习资源
- [Claude Code 从入门到精通](https://www.cnblogs.com/leadingcode/p/19084161) - 综合中文教程
- [Claude Code 完全指南](https://www.cnblogs.com/knqiufan/p/19449849) - 全面详细的使用指南
- [菜鸟教程 Claude Code](http://www.runoob.com/ai-agent/claude-code.html) - 快速入门教程

---

## D. 参考文献

### A 级来源（官方文档）

| ID | 标题 | 来源 |
|----|------|------|
| S001 | Claude Code 官方快速入门 | Anthropic 官方 |
| S002 | Claude Code 概览 | Anthropic 官方 |
| S003 | Claude Code 最佳实践 | Anthropic 官方 |
| S004 | 内存管理官方文档 | Anthropic 官方 |
| S005 | 官方故障排除文档 | Anthropic 官方 |
| S006 | Skills 最佳实践 | Anthropic Platform |
| S007 | Skills 完整指南 PDF | Anthropic 资源 |
| S008 | Skills 和 MCP 扩展博客 | Anthropic 官方博客 |

### B 级来源（权威社区）

| ID | 标题 | 来源 |
|----|------|------|
| S009 | Claude Code 从入门到精通 | 博客园 |
| S010 | 最全 Claude Code 使用指南 | 知乎 |
| S011 | Claude Code 完整使用指南 | GitHub |
| S012 | ClaudeCode 教程中心 | 社区教程 |
| S013 | Claude 中文入门指南 | Claude 中文社区 |
| S014 | Skills 和 MCP Servers 能力 | Medium |
| S015 | MCP Server 完整配置指南 | BrainGrid |
| S016 | MCP 上下文优化 | Scott Spence |
| S017 | Skills vs MCP vs Agents | Colin McNamara |
| S018 | MCP 工具配置指南 | Clockwise |
| S019 | Claude Code 中高级技巧 | 天盘博客 |
| S020 | 多 Claude 协作工作流 | Claude 中文 |
| S021 | Skills 实用指南 | 腾讯新闻 |
| S023 | AI编程实践与协作 | 得物技术 |
| S026 | Claude Code 完全指南 | 博客园 |
| S027 | CLAUDE.md 8条黄金法则 | DAMO开发者 |
| S028 | 从0到1搭建项目环境 | 腾讯云 |
| S029 | Mintlify 文档配置 | Mintlify |
| S030 | Claude 中文网内存管理 | Claude 中文网 |
| S031 | Claude Code Hub 内存 | Community Hub |
| S032 | Skills Fan FAQ | Skills Fan |
| S033 | 完整故障排查指南 | Hrefgo |
| S034 | 架构层面故障排除 | 腾讯云社区 |
| S035 | Claude Code Pro FAQ | ClaudeCode.pro |
| S039 | 多Claude协作教程 | ClaudeCode101 |

### C 级来源（经验分享）

| ID | 标题 | 来源 |
|----|------|------|
| S022 | 高阶用法与骚技巧 | DEV Community |
| S024 | 1小时搞定Web应用 | 腾讯云社区 |
| S025 | B站实战教程 | Bilibili |
| S036 | 傻瓜式调试技巧 | Medium |
| S037 | 高级技巧效率翻倍 | 53AI |
| S038 | 李锋镝高级技巧 | 个人博客 |
| S040 | 菜鸟教程入门 | 菜鸟教程 |

---

> **免责声明**：本报告基于 2026 年 2 月的信息编写。Claude Code 作为一个快速迭代的产品，其功能和配置可能随版本更新而变化。建议读者结合 [官方文档](https://code.claude.com/docs) 获取最新信息。
>
> **报告版本**：V1.0 | **最后更新**：2026-02-10
