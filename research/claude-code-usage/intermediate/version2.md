# Claude Code 使用方式与高效技巧：从入门到精通 (Version 2)

> **版本说明**: 本报告 Version 2 侧重架构思维与设计模式，从工程化角度阐述每个特性的设计理念，并提供对比表格和决策矩阵帮助读者选择合适方案。涵盖 Skills、MCP、Hooks、Subagents 等高级特性的架构分析，以及团队协作和企业级实践视角。
>
> **最后更新**: 2026-02-10 | **适用版本**: Claude Code 最新版

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

Claude Code 是 Anthropic 推出的 **AI 驱动命令行编程助手**，它不是一个简单的代码补全工具，而是一个拥有系统级权限的 Agent 级智能体 [S002, S026]。与传统 IDE 插件不同，Claude Code 直接运行在终端中，能够读写文件、执行 shell 命令、操作 Git 仓库，并通过自然语言与开发者交互完成复杂的编程任务 [S002, S009]。

从架构设计理念来看，Claude Code 采用了 **"终端即操作系统"** 的设计哲学。它将 AI 的推理能力与操作系统的执行能力融合，使得 AI 不再局限于"建议"代码，而是能够"执行"完整的开发工作流 [S026]。这种设计使其天然适合自动化、脚本化的开发场景。

### 1.2 核心特性与优势

Claude Code 的核心竞争力可以从以下四个架构维度理解：

| 特性维度 | 具体能力 | 架构意义 |
|---------|---------|---------|
| **超大上下文窗口** | 200K tokens [S002, S026] | 可一次性理解大型代码库的完整上下文，无需频繁分段处理 |
| **系统级权限** | 文件读写、Shell 执行、Git 操作 [S026] | 从"建议者"升级为"执行者"，实现端到端的任务完成 |
| **三重扩展体系** | Skills / MCP / Hooks [S008, S026] | 分层解耦的扩展架构：知识层(Skills)、工具层(MCP)、流程层(Hooks) |
| **多代理协作** | Subagents / 多终端 [S020, S026] | 支持任务并行化与职责分离，适合复杂工程项目 |

**与传统 AI 编程工具对比：**

| 对比维度 | 传统 AI 插件(如 Copilot) | Claude Code |
|---------|------------------------|-------------|
| 运行环境 | IDE 插件 | 独立终端 Agent |
| 操作权限 | 代码补全/建议 | 系统级读写执行 |
| 上下文范围 | 当前文件/相邻文件 | 200K tokens 全项目 |
| 扩展能力 | 有限 | Skills + MCP + Hooks |
| 自动化程度 | 辅助编写 | 端到端任务完成 |

### 1.3 适用场景

基于 Claude Code 的架构特性，以下场景最能发挥其优势 [S003, S019]：

- **代码理解与问答**：快速理解陌生代码库的架构和逻辑，特别适合接手遗留项目 [S019]
- **大规模重构**：跨文件的批量修改、API 迁移、框架升级等需要全局上下文的任务 [S019]
- **调试与错误修复**：结合执行能力，可以自动运行测试、定位问题、生成修复方案 [S019, S036]
- **测试驱动开发**：先编写测试用例，再让 Claude Code 生成通过测试的实现代码 [S003, S012]
- **Git 工作流自动化**：自动生成 commit message、创建 PR、处理 code review [S009]
- **CI/CD 管道集成**：以 headless 模式嵌入自动化流水线 [S012]

---

## 2. 安装与配置

### 2.1 系统要求

Claude Code 的运行环境要求如下 [S001, S033]：

| 要求项 | 最低版本 | 推荐版本 | 说明 |
|-------|---------|---------|------|
| **Node.js** | 18.0+ | 20 LTS+ | 核心运行时依赖 [S001, S033] |
| **操作系统** | macOS / Linux | macOS 14+ / Ubuntu 22+ | Windows 需通过 WSL2 [S005] |
| **网络** | 稳定互联网 | 低延迟连接 | 需要访问 Anthropic API |
| **磁盘空间** | 500MB | 1GB+ | 包含依赖和缓存 |

### 2.2 安装方法

Claude Code 提供三种安装方式，适应不同环境需求：

**方法一：原生安装（推荐）** [S001]

```bash
# macOS / Linux 一键安装
curl -fsSL https://claude.ai/install.sh | bash
```

这是官方推荐的安装方式，会自动处理依赖和 PATH 配置。

**方法二：Homebrew 安装（macOS）** [S001]

```bash
# 通过 Homebrew
brew install claude-code
```

适合习惯使用 Homebrew 管理工具的 macOS 用户。

**方法三：npm 全局安装** [S001, S009]

```bash
# npm 全局安装
npm install -g @anthropic-ai/claude-code
```

适合 Node.js 开发者，便于版本管理和更新。

**安装方式选择决策：**

| 场景 | 推荐方式 | 理由 |
|------|---------|------|
| macOS 首次安装 | 原生安装 | 自动配置，零门槛 |
| macOS Homebrew 用户 | Homebrew | 统一包管理 |
| Linux 服务器 | npm | 依赖 Node.js 环境通常已有 |
| CI/CD 环境 | npm | 可锁定版本，可重复构建 |
| Windows | WSL2 + 原生安装 | 必须通过 WSL2 [S005] |

### 2.3 首次配置

安装完成后，需要完成初始化配置：

```bash
# 启动 Claude Code（首次运行会引导登录）
claude

# 使用 API Key 方式
export ANTHROPIC_API_KEY="sk-ant-..."
claude
```

认证方式有两种 [S001, S032]：

1. **Anthropic 账户登录**：通过浏览器 OAuth 认证，适合个人使用
2. **API Key 认证**：设置 `ANTHROPIC_API_KEY` 环境变量，适合自动化场景和 CI/CD

> **提示**：如果浏览器未自动打开，可手动复制终端输出的 URL 在浏览器中完成登录 [S032]。

### 2.4 验证安装

```bash
# 检查版本
claude --version

# 运行快速测试
claude "请介绍一下你自己"

# 检查可用工具
claude /help
```

如果以上命令均正常响应，说明安装配置成功 [S001]。若遇到 `command not found` 错误，请检查 PATH 配置（详见第13章问题排查）。

---

# 第二部分：核心功能详解

## 3. 基础使用

### 3.1 启动会话

Claude Code 以项目目录为工作上下文，启动方式灵活 [S001, S010]：

```bash
# 在当前项目目录启动
claude

# 指定项目目录启动
claude --project /path/to/project

# 恢复上次会话
claude --resume

# 以管道方式传入任务（适合脚本化）
echo "分析这个项目的架构" | claude

# 单次任务模式（执行完即退出）
claude -p "生成 README.md"
```

**会话管理的设计理念**：Claude Code 的会话是有状态的，每次对话都基于之前的上下文累积。`--resume` 参数允许恢复中断的长任务，这在大型重构等耗时场景中尤为重要 [S010]。

### 3.2 基本命令

Claude Code 使用**斜线命令 (Slash Commands)** 作为内置操作的快捷入口 [S009, S022]：

| 命令 | 功能 | 使用场景 |
|------|------|---------|
| `/help` | 显示帮助信息 | 查看所有可用命令 [S001] |
| `/memory` | 编辑 CLAUDE.md | 管理项目记忆 [S004, S030] |
| `/terminal-setup` | 终端配置 | 优化终端显示效果 [S019] |
| `/vim` | Vim 模式 | 切换 Vim 键绑定 [S019] |
| `/clear` | 清除上下文 | 释放上下文窗口空间 |
| `/compact` | 压缩上下文 | 保留关键信息，减少 token 用量 |
| `/cost` | 显示费用 | 查看当前会话的 token 消耗 |
| `/logout` | 退出登录 | 切换账户或重新认证 [S032] |

### 3.3 文件操作工具

Claude Code 内置了四种核心文件操作工具，这是其"执行能力"的基石 [S003, S036]：

**Read** -- 读取文件内容

```
> 读取 src/index.ts 的内容
# Claude 会使用 Read 工具获取文件内容并展示
```

**Write** -- 创建或覆盖文件

```
> 创建一个新的 utils/helper.ts 工具函数文件
# Claude 会使用 Write 工具生成完整文件
```

**Edit** -- 精确编辑文件

```
> 将 config.ts 中的 API 端口从 3000 改为 8080
# Claude 会使用 Edit 工具进行精确的文本替换
```

**Glob** -- 文件搜索与模式匹配

```
> 找出所有的 TypeScript 测试文件
# Claude 会使用 Glob 工具匹配 **/*.test.ts 或 **/*.spec.ts
```

**工具调用的架构理解**：这四个工具构成了 Claude Code 的 **IO 层**。每次文件操作都有明确的权限确认机制（在 default 模式下），确保 AI 不会在未经用户允许的情况下修改文件。这种设计平衡了自动化效率与安全性 [S003]。

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
├── 是 → 你了解即将执行的任务吗？
│   ├── 了解 → acceptEdits 模式
│   └── 不了解 → plan 模式先规划
└── 否 → 是 CI/CD 环境吗？
    ├── 是 → dangerously-skip-permissions
    └── 否 → default 模式（学习和原型开发）
```

```bash
# 启动时指定权限模式
claude --permission-mode acceptEdits

# 在 settings.json 中永久配置
# ~/.claude/settings.json
```

---

## 4. 工作模式

### 4.1 Default 模式

Default 模式是 Claude Code 的标准交互模式。在此模式下，Claude 会在执行每个有副作用的操作前请求用户确认 [S019, S010]。

**工作流程**：
1. 用户输入自然语言指令
2. Claude 分析需求，规划执行步骤
3. 对于读取操作，自动执行
4. 对于写入/执行操作，展示计划并等待确认
5. 用户确认后执行，展示结果

**适用场景**：日常开发、代码审查、学习探索。这是新手推荐使用的模式，可以充分了解 Claude 的每一步操作 [S003]。

### 4.2 Auto 自动模式（YOLO 模式）

Auto 模式允许 Claude 自动执行所有操作而无需逐步确认，也被社区称为"YOLO 模式" [S024]。

```bash
# 以 Auto 模式启动
claude --dangerously-skip-permissions

# 或在已配置的 allowlist 下使用 acceptEdits 模式
claude --permission-mode acceptEdits
```

**适用场景** [S024]：
- 快速原型开发：需要快速迭代，不在乎中间过程
- CI/CD 流水线：自动化场景中没有人工交互的可能
- 沙盒环境：在 Docker 容器等隔离环境中安全运行

**风险控制建议**：
- 在 Git 仓库中使用，确保可以随时回退 [S003]
- 设置文件操作的白名单/黑名单
- 在隔离环境（Docker、VM）中运行
- 不要在包含敏感数据的目录中使用

### 4.3 Plan 计划模式

Plan 模式是一种"只读分析"模式，Claude 可以阅读代码和推理方案，但不会做任何实际修改 [S019, S027]。

**设计理念**：Plan 模式体现了"先思考，后行动"的软件工程原则。在复杂任务中，先用 Plan 模式生成详细的实施方案，审核通过后再切换到执行模式，可以大幅减少返工 [S003]。

**使用流程**：

```
步骤1：Plan 模式生成方案
> [Plan 模式] 分析如何将此项目从 REST API 迁移到 GraphQL

步骤2：审核方案（Claude 输出详细的迁移计划）

步骤3：切换到 Default/Auto 模式执行
> [Default 模式] 按照刚才的计划执行迁移
```

### 4.4 模式切换技巧

Claude Code 支持在会话中动态切换模式 [S024]：

- **Shift+Tab**：在输入区域快速切换工作模式
- **斜线命令**：通过 `/` 开头的命令切换特定功能

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

`CLAUDE.md` 是 Claude Code 的**项目记忆文件**，它决定了 Claude 如何理解你的项目、遵循什么规范、采用什么工作流 [S004, S019, S027]。可以将其类比为"给 AI 的项目手册"--每次 Claude Code 启动时都会读取并遵循其中的指令。

### 5.2 三层内存架构

Claude Code 采用了精心设计的三层内存架构，从组织到个人逐级细化 [S004, S030]：

```
┌─────────────────────────────────────┐
│  企业策略层 (Enterprise Policy)       │  ← 组织管理员配置
│  路径: 管理控制台设定                   │  ← 强制执行，不可覆盖
├─────────────────────────────────────┤
│  项目内存层 (Project Memory)          │  ← 团队共享
│  路径: {项目根}/CLAUDE.md             │  ← 提交到 Git，版本控制
│       {项目根}/.claude/rules/*.md    │  ← 模块化规则文件
├─────────────────────────────────────┤
│  用户内存层 (User Memory)             │  ← 个人偏好
│  路径: ~/.claude/CLAUDE.md           │  ← 跨项目生效
│       ~/.claude/rules/*.md          │  ← 个人规则
└─────────────────────────────────────┘
```

**加载优先级**（从高到低）[S004]：
1. 企业策略 -- 组织级强制规则
2. 用户 CLAUDE.md -- 个人全局偏好
3. 项目 CLAUDE.md -- 团队共享规范
4. `.claude/rules/` 目录下的规则文件 -- 模块化补充规则
5. 子目录 CLAUDE.md -- 特定目录的局部规则

**架构设计理念**：这种分层结构借鉴了 CSS 的级联优先级机制。企业策略类似于 `!important`，不可被下层覆盖；而项目和用户层可以互相补充。这使得大型团队可以在保持统一规范的同时，允许个人定制 [S004, S030]。

### 5.3 CLAUDE.md 编写最佳实践 -- 8 条黄金法则

基于社区实践和官方建议，总结出以下 8 条编写黄金法则 [S027]：

**法则1：保持简短精炼**

```markdown
# 好的示例
- 使用 TypeScript strict 模式
- 函数参数不超过 3 个
- 使用 Prettier 格式化（2空格缩进）

# 避免的示例
- 在所有文件中，我们需要确保使用 TypeScript 的严格模式来进行类型检查，
  这样可以帮助我们在编译阶段发现更多的类型错误...（冗长描述）
```

CLAUDE.md 的内容会消耗上下文窗口的 token，因此应当用最精炼的语言传递最关键的信息 [S027, S004]。

**法则2：使用清晰的结构**

采用 Markdown 标题层级组织内容，使 Claude 能快速定位相关规则 [S027]。

**法则3：避免冗余信息**

不要在 CLAUDE.md 中放置 Claude 已经知道的通用知识（如编程语言语法）[S027]。

**法则4：具体而非抽象**

```markdown
# 好的示例
- 错误处理使用 Result<T, Error> 模式，不要用 try-catch
- API 响应格式: { data: T, error: string | null, code: number }

# 避免的示例
- 请写出高质量的代码
- 请注意错误处理
```

**法则5：包含关键上下文**

告诉 Claude 项目的核心信息，如技术栈、架构模式、关键依赖 [S027]。

**法则6：定期更新维护**

随项目演进更新 CLAUDE.md，删除过时信息，补充新的规范 [S027]。

**法则7：使用导入机制**

将大型规则集拆分为独立文件，通过 `@import` 引用 [S027, S004]。

**法则8：分离公私配置**

团队规范放在项目 CLAUDE.md（提交到 Git），个人偏好放在用户 CLAUDE.md [S027]。

### 5.4 推荐的 CLAUDE.md 模板

```markdown
# 项目概述
- 项目名称: MyApp
- 技术栈: React 18 + TypeScript + Vite + Tailwind CSS
- 后端: Node.js + Express + PostgreSQL
- 架构: 前后端分离, 微服务雏形

# 目录结构
- src/components/ - React 组件
- src/hooks/ - 自定义 Hooks
- src/services/ - API 服务层
- src/utils/ - 工具函数
- server/ - 后端服务

# 编码规范
- 使用 TypeScript strict 模式
- 组件使用函数式写法 + Hooks
- 样式使用 Tailwind CSS，不写自定义 CSS
- API 请求统一使用 src/services/ 中的封装
- 错误处理使用 Result 模式

# Git 规范
- commit message 遵循 Conventional Commits
- 分支命名: feature/xxx, fix/xxx, chore/xxx

# 测试规范
- 使用 Vitest 作为测试框架
- 组件测试使用 @testing-library/react
- 覆盖率目标: 80%+

# 导入额外文档
@docs/api-guidelines.md
@docs/database-schema.md
```

### 5.5 导入与继承机制

`@import` 语法让 CLAUDE.md 支持模块化组织 [S004, S030]：

```markdown
# 在 CLAUDE.md 中导入其他文件
@docs/coding-standards.md
@docs/api-reference.md
@.claude/rules/testing-rules.md
```

**关键规则** [S004, S030]：
- 支持相对路径和绝对路径
- 最大递归深度为 **5 层**，防止无限循环
- 系统会自动检测并阻止循环引用
- 导入的文件内容会展开到当前位置

### 5.6 模块化规则 (.claude/rules/)

对于大型项目，建议将规则按职责拆分到 `.claude/rules/` 目录 [S004]：

```
.claude/
└── rules/
    ├── code-style.md      # 代码风格规则
    ├── testing.md          # 测试规范
    ├── git-workflow.md     # Git 工作流
    ├── api-design.md       # API 设计规范
    └── security.md         # 安全检查规则
```

规则文件支持 **Glob 模式匹配**，使规则只在特定文件类型上生效 [S004]：

```markdown
<!-- .claude/rules/react-rules.md -->
<!-- globs: src/components/**/*.tsx -->

# React 组件规范
- 每个组件单独一个文件
- Props 使用 interface 定义，不用 type
- 使用 memo 包裹纯展示组件
```

### 5.7 内存优化策略

**快速添加记忆** [S004, S030]：
- 在对话中使用 `#` 快捷方式即可将规则直接追加到 CLAUDE.md
- 使用 `/memory` 命令打开编辑器进行批量编辑

**内存查找机制**：Claude Code 启动时会从当前目录向上递归查找 CLAUDE.md 文件，直到文件系统根目录 [S004, S030, S031]。这意味着你可以在 monorepo 中为每个子包设置独立的规则文件。

**内存清理**：定期审查 CLAUDE.md，移除过时规则，保持文件精简。过大的 CLAUDE.md 会浪费上下文窗口空间 [S003, S033]。

---

# 第三部分：高级特性

## 6. Skills 技能系统

### 6.1 概念与架构

Skills 是 Claude Code 的**知识扩展层**，本质上是一组结构化的 Markdown 指令文件，用于教会 Claude 特定领域的工作流程和最佳实践 [S006, S007, S008]。

**架构定位**：如果将 Claude Code 比作一个操作系统，那么 Skills 就是安装在其上的"应用程序知识库"。它不提供新的工具能力（那是 MCP 的职责），而是提供"如何使用现有工具完成特定任务"的专家知识 [S017]。

```
Claude Code 扩展架构
├── Skills（知识层）    → "知道怎么做" → Markdown 指令文件
├── MCP（工具层）       → "能做什么"   → 外部工具/API 连接
├── Hooks（流程层）     → "何时触发"   → 事件驱动的自动化
└── Subagents（协作层）  → "谁来做"    → 任务分派与并行执行
```

**Skills vs MCP vs Hooks 对比**：

| 维度 | Skills | MCP | Hooks |
|------|--------|-----|-------|
| 本质 | 知识/工作流指令 | 工具/API 连接 | 事件触发脚本 |
| 实现方式 | Markdown 文件 | JSON 配置 + Server 进程 | Shell 脚本/可执行文件 |
| 扩展方向 | 教 Claude "怎么做" | 给 Claude "新工具" | 在 Claude 动作前后"自动执行" |
| Token 消耗 | 按需加载，较低 | 工具描述常驻，较高 | 无 token 消耗 |
| 可移植性 | 高（纯文本） | 中（需安装 Server） | 中（依赖系统环境） |
| 开发门槛 | 低（写 Markdown） | 中（配置 JSON + 依赖） | 中（编写脚本） |

[S017]

### 6.2 官方 Skills 推荐

以下是社区公认最实用的 Skills [S021]：

**Superpowers**（1.6 万+ Star）
- 功能：增强 Claude 的系统级能力，包括更好的规划、代码审查、重构策略
- 安装：`/install-skill https://github.com/anthropics/claude-code-superpowers`
- 适用：所有项目，建议作为基础 Skill 安装

**Planning-with-files**
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

### 6.3 安装与管理

```bash
# 安装官方或社区 Skill
/install-skill <skill-url>

# 查看已安装的 Skills
# Skills 安装后位于 .claude/skills/ 目录

# 手动安装（将 Skill 文件放入指定目录）
mkdir -p .claude/skills/my-skill
```

Skills 的安装本质上是将包含 `SKILL.md` 的目录放置到项目的 `.claude/skills/` 中 [S007, S012]。

### 6.4 自定义 Skills 开发

开发自定义 Skill 的核心是编写 `SKILL.md` 文件 [S006, S007]：

**目录结构**：

```
my-custom-skill/
├── SKILL.md              # 核心指令文件（必须）
├── resources/            # 参考资源文件（可选）
│   ├── templates/        # 模板文件
│   └── examples/         # 示例代码
└── README.md             # Skill 说明（可选）
```

**SKILL.md 编写规范** [S006, S007]：

```markdown
# Code Review Skill

## 触发条件
当用户请求代码审查或使用 /review 命令时激活此 Skill。

## 审查流程

### 步骤1：安全检查
- 检查是否存在硬编码的密钥或凭证
- 验证输入校验是否完备
- 检查 SQL 注入、XSS 等安全漏洞

### 步骤2：代码质量检查
- 函数复杂度是否过高（圈复杂度 > 10 需警告）
- 是否遵循单一职责原则
- 命名是否清晰、有意义

### 步骤3：性能检查
- 是否有 N+1 查询问题
- 是否有不必要的重渲染
- 大数据集是否有分页处理

### 步骤4：输出格式
输出格式为：
- 🔴 严重问题（必须修复）
- 🟡 建议改进（推荐修复）
- 🟢 良好实践（值得表扬）

## 约束
- 不自动修改代码，只输出审查报告
- 每个问题都要给出具体的改进建议
- 标注问题所在的文件名和行号
```

**Skills 开发最佳实践** [S006, S007]：

1. **单一职责**：每个 Skill 聚焦一个特定任务，不要做"万能 Skill"
2. **步骤明确**：将复杂任务分解为清晰的步骤序列
3. **输出格式化**：定义明确的输出格式，便于后续处理
4. **可测试性**：Skill 的效果应该可以通过具体案例验证
5. **示例充足**：在 `resources/examples/` 中提供充分的输入输出示例

### 6.5 实战案例：创建测试生成 Skill

```markdown
# Test Generator Skill

## 目标
根据给定的源代码文件，自动生成全面的单元测试。

## 执行步骤

### 1. 分析源文件
- 识别所有公共函数和方法
- 分析参数类型和返回类型
- 识别边界条件和异常路径

### 2. 生成测试用例
对每个函数生成以下类别的测试：
- 正常路径测试（Happy Path）
- 边界值测试（Boundary）
- 异常输入测试（Error Cases）
- 空值/未定义测试（Null/Undefined）

### 3. 测试框架
- 使用项目已配置的测试框架（从 package.json 或 CLAUDE.md 获取）
- 默认使用 Vitest
- 遵循 AAA 模式（Arrange-Act-Assert）

### 4. 输出
- 测试文件放置在源文件同目录的 __tests__/ 下
- 文件名: {source}.test.{ext}
- 运行测试确认全部通过
```

---

## 7. MCP Server 集成

### 7.1 MCP 协议介绍

**MCP（Model Context Protocol）** 是 Anthropic 推出的开放协议，旨在标准化 AI 模型与外部工具、数据源之间的连接方式 [S008, S015]。它被社区形象地称为"AI 的 USB-C 接口"--就像 USB-C 统一了设备充电和数据传输标准一样，MCP 统一了 AI 调用外部工具的方式 [S018]。

**MCP 在 Claude Code 中的角色**：

```
用户请求 → Claude 推理 → 选择 MCP 工具 → 调用外部服务 → 处理结果 → 返回用户
                              ↓
                    ┌─────────────────┐
                    │  MCP Server 1   │ ← 数据库查询
                    │  MCP Server 2   │ ← 网页搜索
                    │  MCP Server 3   │ ← 文件系统
                    │  MCP Server N   │ ← 自定义服务
                    └─────────────────┘
```

**Skills vs MCP 的协作关系**：Skills 告诉 Claude "如何完成任务的知识"，MCP 给 Claude "实际执行任务的工具"。例如，一个数据库迁移 Skill 知道迁移的最佳实践步骤，而 Supabase MCP Server 提供了实际执行 SQL 的能力。两者组合使用效果最佳 [S008]。

### 7.2 配置方法

MCP Server 通过 JSON 配置文件进行管理 [S015, S018]：

**项目级配置**（推荐，团队共享）：

```json
// .claude/mcp.json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${BRAVE_API_KEY}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**用户级配置**（个人偏好，跨项目）：

```json
// ~/.claude/mcp.json
{
  "mcpServers": {
    "my-database": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "https://xxx.supabase.co",
        "SUPABASE_KEY": "${SUPABASE_KEY}"
      }
    }
  }
}
```

> **安全提示**：将 API 密钥存储在环境变量中，使用 `${VAR}` 语法引用，不要在配置文件中硬编码密钥 [S015]。

### 7.3 常用 MCP Servers

| MCP Server | 功能 | 典型用途 | 配置复杂度 |
|-----------|------|---------|-----------|
| **Brave Search** | 网页搜索 | 查询最新技术文档、API 参考 [S018] | 低 |
| **Supabase** | 数据库操作 | 查询/修改数据、管理表结构 [S015] | 中 |
| **GitHub** | 代码仓库 | PR 管理、Issue 处理、代码搜索 [S015] | 低 |
| **Figma** | 设计文件 | 提取设计稿尺寸、颜色、组件信息 [S015] | 中 |
| **Playwright** | 浏览器自动化 | E2E 测试、网页截图、表单填写 [S015] | 中 |
| **Filesystem** | 增强文件操作 | 跨目录文件管理、批量操作 | 低 |
| **Sentry** | 错误监控 | 查看和分析生产环境错误 | 中 |

**MCP Server 选择决策**：

```
你需要什么额外能力？
├── 搜索互联网信息  → Brave Search
├── 操作数据库     → Supabase / PostgreSQL MCP
├── 管理 GitHub    → GitHub MCP
├── 从设计稿开发   → Figma MCP
├── 浏览器测试     → Playwright MCP
├── 监控生产错误   → Sentry MCP
└── 自定义需求     → 开发自定义 MCP Server
```

### 7.4 上下文优化（Token 消耗问题）

MCP Server 的一个重要注意事项是**工具描述的 token 消耗** [S016]。每个已启用的 MCP Server 的工具描述都会占用上下文窗口空间。根据实测数据，某些配置可能消耗超过 66,000 tokens 仅用于工具描述 [S016]。

**优化策略** [S016]：

1. **按需启用**：只启用当前任务需要的 MCP Server，不要"装了不用"
2. **精简配置**：审查每个 Server 暴露的工具数量，禁用不必要的工具
3. **监控消耗**：使用 `/cost` 命令定期检查 token 使用情况
4. **项目隔离**：使用项目级配置而非全局配置，避免无关工具加载

```bash
# 查看当前上下文使用情况
/cost

# 禁用特定 MCP Server（删除或注释配置）
# 建议：开发前端时禁用数据库相关 MCP
# 建议：调试时禁用搜索相关 MCP
```

### 7.5 实战案例：数据库驱动开发

以 Supabase MCP 为例，展示完整的数据库集成工作流：

```json
// .claude/mcp.json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "${SUPABASE_URL}",
        "SUPABASE_KEY": "${SUPABASE_KEY}"
      }
    }
  }
}
```

配置完成后，可以直接用自然语言操作数据库：

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

Hooks 是 Claude Code 的**流程自动化层**，采用事件驱动机制，在 Claude 执行特定操作的前后自动触发预定义的脚本 [S009, S022]。

**事件模型**：

```
用户输入 → [PreToolUse Hook] → Claude 工具调用 → [PostToolUse Hook] → 结果返回
                                                                         ↓
                                                              [Notification Hook]
```

**支持的 Hook 事件类型** [S009, S022]：

| 事件类型 | 触发时机 | 典型用途 |
|---------|---------|---------|
| `PreToolUse` | 工具调用前 | 参数校验、权限检查、格式化 |
| `PostToolUse` | 工具调用后 | 结果处理、日志记录、通知 |
| `Notification` | Claude 发送通知时 | 自定义提醒方式 |
| `Stop` | Claude 完成响应时 | 后处理、统计、清理 |

### 8.2 常用钩子场景

**场景一：提交前自动检查** [S009]

```json
// .claude/settings.json
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

**场景二：代码格式化** [S009]

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npx prettier --write $CLAUDE_FILE_PATH",
        "description": "文件修改后自动格式化"
      }
    ]
  }
}
```

**场景三：测试自动运行** [S009]

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(src/**/*.ts)",
        "command": "npx vitest run --reporter=verbose $CLAUDE_FILE_PATH",
        "description": "源文件修改后自动运行相关测试"
      }
    ]
  }
}
```

**场景四：自定义通知** [S022]

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "osascript -e 'display notification \"Claude 已完成任务\" with title \"Claude Code\"'",
        "description": "任务完成后发送 macOS 通知"
      }
    ]
  }
}
```

### 8.3 自定义 Hook 开发

开发自定义 Hook 的要点 [S022]：

1. **脚本位置**：Hook 脚本可以是任意可执行文件，建议统一放在 `.claude/hooks/` 目录
2. **环境变量**：Hook 脚本可以访问 Claude 注入的环境变量（如 `$CLAUDE_FILE_PATH`）
3. **退出码**：返回 0 表示成功，非 0 会阻止后续操作（PreToolUse 场景）
4. **超时控制**：建议在脚本中设置超时，避免长时间阻塞

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
| **错误宽容** | 非关键 Hook 不应阻断流程 | 使用 `|| true` 避免意外中断 [S009] |
| **日志记录** | 记录 Hook 的执行状态 | 便于调试和审计 [S009] |
| **幂等性** | Hook 重复执行结果一致 | 避免副作用累积 |
| **安全意识** | 不在 Hook 中硬编码密钥 | 使用环境变量传递敏感信息 |

---

## 9. Subagents 子代理

### 9.1 协作模式概述

Subagents（子代理）是 Claude Code 的**任务协作层**，允许主 Claude 实例将子任务分派给独立的 Claude 子进程处理 [S017, S020, S023]。这种设计模式借鉴了操作系统中的进程模型--主进程负责调度，子进程负责执行。

**Subagents 的架构定位**：

| 层级 | 角色 | 工作方式 |
|------|------|---------|
| **主 Agent** | 任务编排者 | 分析需求、拆分任务、汇总结果 |
| **Sub Agent** | 任务执行者 | 独立上下文、聚焦单一任务 |
| **Task Tool** | 分派机制 | 主 Agent 用来创建和管理子任务 |

### 9.2 并行任务处理（Task Tool）

Task Tool 是 Claude Code 内置的子任务分派工具 [S020, S036]：

```
主 Agent 接收任务: "为这个项目添加用户认证系统"
    │
    ├── Task 1: "分析现有的用户模型和数据库结构"
    ├── Task 2: "设计 JWT 认证流程"
    ├── Task 3: "编写认证中间件"
    └── Task 4: "生成相关的单元测试"
```

**任务分解策略** [S023]：

1. **上下文隔离原则**：每个子任务应该有独立的、最小化的上下文。子 Agent 不会继承主 Agent 的完整对话历史，这既减少了 token 消耗，也避免了无关信息干扰 [S020, S023]。

2. **任务粒度控制**：子任务应足够具体，能在单次交互中完成。过大的子任务会导致子 Agent 需要过多上下文；过小的子任务会增加调度开销。

3. **结果聚合**：主 Agent 负责收集所有子任务的结果，进行冲突检测和整合。

### 9.3 多 Claude 协作工作流

除了 Task Tool 的自动子代理外，开发者还可以手动在多个终端中运行 Claude 实例，形成协作工作流 [S020, S039]：

**模式一：编码与验证分离**

```
终端 1 (编码 Claude):
$ claude
> 实现用户注册功能，包括邮箱验证

终端 2 (审查 Claude):
$ claude
> 审查 src/auth/ 目录下最近的代码变更，检查安全性

终端 3 (测试 Claude):
$ claude
> 为 src/auth/ 下的所有模块编写并运行测试
```

**模式二：测试驱动协作 (TDD)** [S020, S039]

```
终端 1 (测试 Claude):
$ claude
> 根据需求文档，为用户认证模块编写全面的测试用例
> 测试应该全部失败（因为实现还不存在）

终端 2 (实现 Claude):
$ claude
> 查看 tests/auth/ 下的测试用例，编写代码使所有测试通过
```

**模式三：前后端并行开发**

```
终端 1 (后端 Claude):
$ claude
> 实现 RESTful API：用户 CRUD 和认证接口
> 生成 OpenAPI 规范文档

终端 2 (前端 Claude):
$ claude
> 根据 docs/api-spec.yaml 实现前端页面和 API 调用
```

**并行开发注意事项** [S020, S023]：

| 风险 | 应对策略 |
|------|---------|
| 文件冲突 | 明确每个 Claude 实例的文件操作范围，避免同时修改同一文件 |
| 接口不一致 | 先定义接口契约（API 规范、TypeScript 类型），再并行开发 |
| 上下文发散 | 使用共享的 CLAUDE.md 确保所有实例遵循统一规范 |
| 资源竞争 | 避免多个实例同时运行数据库迁移等排他操作 |

### 9.4 企业团队协作实践

得物技术团队的 AI 编程实践提供了企业级 Subagent 应用的参考案例 [S023]：

**对话流设计方法论**：
- 将复杂业务需求拆解为多轮对话流
- 每轮对话聚焦一个明确的子目标
- 通过上下文传递机制保持对话连贯性

**子代理系统设计思考**：
- 为不同职责的 Agent 配置专属的 CLAUDE.md 规则
- 建立 Agent 间的通信协议（通过文件系统或共享目录）
- 在代码审查环节引入独立的"审查 Agent"，实现人机混合审查

**团队规范统一**：
- 企业级 CLAUDE.md 作为基线规范，确保所有开发者的 Claude 行为一致
- 定期同步 CLAUDE.md 更新，纳入团队的代码评审流程
- 积累团队级 Skills 库，将最佳实践沉淀为可复用的 Skill

---

# 第四部分：工作流优化

## 10. 高效工作流

### 10.1 探索-规划-编码（Explore-Plan-Code）流程

这是 Claude Code 官方推荐的核心工作流，体现了"三思而后行"的工程理念 [S003, S012, S019]：

**阶段一：探索（Explore）**

```
> 帮我理解这个项目的整体架构
> 分析 src/ 目录下的核心模块及其依赖关系
> 这个项目使用了哪些设计模式？
```

在此阶段，Claude 会使用 Read 和 Glob 工具遍历项目文件，建立对代码库的全局理解。推荐使用 Plan 模式，确保这个阶段只读不写 [S003]。

**阶段二：规划（Plan）**

```
> 我需要添加用户权限管理功能，请制定详细的实施计划
> 列出需要修改的文件、新建的文件、以及修改顺序
> 评估每个步骤的风险和注意事项
```

Claude 会基于探索阶段的理解，输出结构化的实施方案。此阶段仍建议使用 Plan 模式 [S003, S019]。

**阶段三：编码（Code）**

```
> 按照刚才的计划，开始实现第1步：创建权限模型
> 实现完成后，运行测试确认
```

切换到 Default 或 acceptEdits 模式，逐步执行计划。建议每完成一个步骤就提交一次，便于回退 [S003]。

**三阶段工作流总结**：

| 阶段 | 推荐模式 | 主要操作 | 产出物 |
|------|---------|---------|--------|
| 探索 | Plan | Read, Glob, Grep | 架构理解、依赖图谱 |
| 规划 | Plan | 推理、分析 | 实施计划、风险评估 |
| 编码 | Default/acceptEdits | Write, Edit, Bash | 代码、测试、文档 |

### 10.2 测试驱动开发（TDD）

Claude Code 天然适合 TDD 工作流，因为它可以同时编写测试和实现代码 [S003, S012, S019]：

**Red-Green-Refactor 循环**：

```
步骤 1 (Red): 编写失败的测试
> 为 UserService.createUser() 方法编写全面的测试用例
> 包括正常流程、重复用户名、无效邮箱等场景
> （此时测试应该全部失败）

步骤 2 (Green): 让测试通过
> 查看刚才编写的测试用例
> 实现 UserService.createUser() 使所有测试通过
> 运行测试确认全部通过

步骤 3 (Refactor): 重构优化
> 审查刚才的实现代码
> 在保持测试通过的前提下，优化代码质量
> 提取公共逻辑、改进命名、简化复杂度
```

**TDD 最佳实践** [S003, S012]：
- 让 Claude 先读取项目的测试框架配置，确保生成兼容的测试代码
- 测试用例应覆盖正常路径、边界条件、异常情况
- 每次只关注一个功能点，避免测试范围过大
- 利用 `claude --resume` 保持 TDD 循环的上下文连续性

### 10.3 Git 集成最佳实践

Claude Code 深度集成了 Git 工作流 [S009]：

**自动提交（commit）**

```bash
# 在 Claude Code 中
> /commit
# 或使用自然语言
> 将当前的修改提交，生成符合 Conventional Commits 规范的 commit message
```

Claude 会自动分析变更内容，生成语义化的 commit message [S009]：

```
feat(auth): add JWT-based user authentication

- Implement UserService with createUser/login methods
- Add JWT token generation and validation middleware
- Create auth routes with proper error handling
```

**创建 Pull Request**

```bash
> 为当前分支创建 PR，目标分支为 main
# Claude 会自动生成 PR 标题和描述，包含变更摘要
```

**基于 Issue 修复**

```bash
> 修复 GitHub Issue #42: 用户登录后 session 过期时间不正确
# Claude 会读取 Issue 内容，分析问题，生成修复代码
```

### 10.4 CI/CD 自动化

Claude Code 支持 Headless 模式，可嵌入 CI/CD 流水线 [S012]：

```yaml
# GitHub Actions 示例
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "审查这个 PR 的代码变更，检查安全性和代码质量问题" \
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

掌握以下快捷键可以显著提升操作效率 [S019, S024]：

| 快捷键 | 功能 | 使用场景 |
|--------|------|---------|
| **Esc Esc**（双击 Escape） | 编辑上一条消息 | 修正拼写、补充细节 [S019] |
| **Shift+Tab** | 切换工作模式 | 在 Plan/Default/Auto 间切换 [S024] |
| **Shift+Enter** | 多行输入 | 编写复杂的多行指令 [S019] |
| **#** | 快速添加记忆 | 将规则追加到 CLAUDE.md [S004, S030] |
| **Tab** | 接受建议/补全 | 快速确认 Claude 的建议 |
| **Ctrl+C** | 中断当前操作 | 停止长时间运行的任务 |
| **Ctrl+D** | 退出会话 | 结束当前 Claude Code 会话 |

### 11.2 IDE 集成

**VS Code 集成** [S012, S022]

Claude Code 可以与 VS Code 终端无缝配合：

```bash
# 在 VS Code 内置终端中启动
claude

# 配置 VS Code 终端快捷键
# settings.json
{
  "terminal.integrated.defaultProfile.osx": "zsh",
  "terminal.integrated.env.osx": {
    "ANTHROPIC_API_KEY": "${env:ANTHROPIC_API_KEY}"
  }
}
```

**VS Code 使用技巧**：
- 在 VS Code 终端中运行 Claude Code，可以同时利用 IDE 的文件树和编辑器预览变更
- 使用 VS Code 的分割终端功能，一个面板运行 Claude，另一个面板运行测试
- 通过 VS Code 的 Source Control 面板直观查看 Claude 的文件变更

**JetBrains IDEs 集成** [S012, S022]

JetBrains 系列（PyCharm、WebStorm、IntelliJ IDEA 等）同样支持：

```bash
# 在 JetBrains 内置终端中使用
claude

# WSL2 环境下的特殊配置
# 需要将 JetBrains 的 WSL 网络模式设置为 "mirrored"
```

> **注意**：在 WSL2 环境下使用 JetBrains IDE 时，需要确保网络配置正确，否则 Claude Code 可能无法检测到 IDE [S005]。

### 11.3 终端配置优化

**Shell 配置建议** [S019]：

```bash
# ~/.zshrc 或 ~/.bashrc

# Claude Code 别名
alias cc="claude"
alias ccr="claude --resume"
alias ccp="claude -p"

# 快速在当前目录启动 Claude Code
alias ccd="claude --project $(pwd)"

# 设置默认权限模式
export CLAUDE_PERMISSION_MODE="acceptEdits"
```

**终端设置优化**：

```bash
# 运行 Claude Code 的终端配置向导
claude /terminal-setup
```

此命令会自动检测并配置终端的字体、编码、颜色等设置，确保 Claude Code 的输出格式正确显示 [S019]。

### 11.4 批量操作技巧

**多文件批量修改** [S019, S037]：

```
> 将项目中所有 .js 文件重命名为 .ts
> 并更新所有 import 语句的文件扩展名

> 在所有 React 组件中将 class components 迁移为 functional components

> 为 src/services/ 下所有文件添加统一的错误处理封装
```

**项目级搜索替换**：

```
> 将项目中所有使用 moment.js 的地方替换为 dayjs
> 包括 import 语句、API 调用方式、日期格式化写法
```

Claude Code 的 200K 上下文窗口使其能够理解跨文件的依赖关系，在批量修改时保持一致性 [S002, S026]。

---

## 12. 性能优化

### 12.1 上下文窗口管理

Claude Code 拥有 200K tokens 的上下文窗口 [S002, S026]，但高效管理这个窗口对性能至关重要：

**上下文消耗来源分析**：

| 消耗来源 | 估算占比 | 优化空间 |
|---------|---------|---------|
| 系统提示词 | 5-10% | 低（固定消耗） |
| CLAUDE.md 内容 | 2-5% | 中（精简内容） |
| MCP 工具描述 | 10-30% | 高（按需加载） |
| 对话历史 | 30-50% | 高（定期清理） |
| 文件内容 | 20-40% | 高（精确读取） |

**管理策略**：
- 使用 `/compact` 命令压缩当前上下文，保留关键信息 [S003]
- 使用 `/clear` 在任务切换时清除无关上下文
- 在长会话中定期使用 `/cost` 监控 token 消耗
- 避免一次性读取超大文件，使用行号范围精确读取

### 12.2 Token 使用优化

**减少不必要的文件读取** [S003]：

```
# 好的做法：明确告诉 Claude 目标文件
> 读取 src/auth/jwt.ts 的第 20-50 行

# 避免：让 Claude 盲目搜索
> 找到项目中处理 JWT 的代码（可能导致读取大量文件）
```

**使用 Glob 精确匹配** [S003]：

```
# 好的做法：精确的 Glob 模式
> 找到 src/components/ 下所有以 Auth 开头的 .tsx 文件

# 避免：过于宽泛的搜索
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
- 在需要频繁引用的内容上，建议在会话早期读取
- 利用 `--resume` 在多次迭代中复用上下文
- 避免在同一会话中重复要求 Claude 阅读相同文件

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

# 减少 MCP 工具加载
# 只在需要时启用特定 MCP Server

# 使用 -p 单次模式执行简单任务（避免会话开销）
claude -p "将 config.ts 中的端口改为 8080"
```

---

# 第五部分：问题解决

## 13. 常见问题与解决方案

### 13.1 安装类问题

**问题：`claude: command not found`** [S032, S033]

```bash
# 原因：安装目录未加入 PATH

# 解决方案1：将 ~/.local/bin 添加到 PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 解决方案2：使用 npm 全局安装路径
npm config get prefix
# 确认输出路径在 PATH 中

# 验证
which claude
claude --version
```

**问题：Node.js 版本不兼容** [S033, S034]

```bash
# 检查版本
node --version  # 需要 18.0+

# 使用 nvm 管理版本
nvm install 20
nvm use 20
nvm alias default 20

# 验证
node --version
claude --version
```

**问题：WSL 路径冲突** [S005, S033]

```bash
# 问题根因：WSL 使用了 Windows 的 Node.js 而非 Linux 版本

# 检查 Node.js 路径
which node
# 如果输出 /mnt/c/... 说明使用的是 Windows 版

# 解决方案：在 WSL 中安装 Linux 版 Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证
which node  # 应该输出 /usr/bin/node 或 /usr/local/bin/node
```

**问题：权限错误（Permission Denied）** [S005, S032, S033]

```bash
# 避免使用 sudo 安装
# 错误做法
sudo npm install -g @anthropic-ai/claude-code

# 正确做法：修复 npm 全局目录权限
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 重新安装
npm install -g @anthropic-ai/claude-code
```

### 13.2 配置类问题

**问题：API Key 无效** [S032, S035]

排查步骤：
1. 确认 Key 格式正确（以 `sk-ant-` 开头）
2. 检查账户余额是否充足
3. 确认 Key 未过期或被撤销
4. 重新生成 Key 并完整复制（注意首尾空格）

```bash
# 重新设置 API Key
export ANTHROPIC_API_KEY="sk-ant-your-new-key"

# 或使用 OAuth 重新登录
claude /logout
claude  # 触发重新认证
```

**问题：登录状态丢失** [S032]

```bash
# 清除凭证缓存
claude /logout

# 检查凭证存储目录权限
ls -la ~/.claude/

# 重新登录
claude
```

**问题：浏览器未自动打开** [S032]

在某些终端环境中（如 SSH 远程连接、Docker 容器），浏览器无法自动打开。解决方案是手动复制终端输出的认证 URL 到本地浏览器完成登录。

### 13.3 运行时问题

**问题：高 CPU/内存占用** [S005, S034]

```bash
# 排查步骤
# 1. 检查 MCP Server 状态
# 某些 MCP Server 可能存在内存泄漏

# 2. 减少启用的 MCP Server 数量
# 编辑 .claude/mcp.json，注释不需要的 Server

# 3. 限制上下文大小
/compact  # 压缩上下文
/clear    # 清除并开始新会话

# 4. 监控资源使用
top -pid $(pgrep -f "claude")
```

**问题：命令执行挂起/超时** [S005, S034]

```bash
# 1. 检查网络连接
curl -I https://api.anthropic.com

# 2. 设置代理（如果需要）
export HTTPS_PROXY="http://proxy:port"

# 3. 强制中断并重启
# Ctrl+C 中断当前操作
# 如无响应，关闭终端重新启动

# 4. 清除可能损坏的缓存
rm -rf ~/.claude/cache/
```

### 13.4 IDE 集成问题

**问题：JetBrains IDE 未被检测** [S005]

在 WSL2 环境下，需要特殊网络配置：

```bash
# 在 WSL2 配置中设置 mirrored 网络模式
# 编辑 /etc/wsl.conf 或 Windows 的 .wslconfig
[wsl2]
networkingMode=mirrored
```

**问题：VS Code 终端 Escape 键异常** [S005]

```json
// VS Code settings.json
{
  "terminal.integrated.commandsToSkipShell": [
    "-workbench.action.quickOpen"
  ],
  "terminal.integrated.sendKeybindingsToShell": true
}
```

---

## 14. 调试技巧

### 14.1 执行追踪

Claude Code 的对话窗口本身就是一份完整的执行日志 [S036]。每次 Claude 调用工具时，都会显示：

- **工具名称**：如 Read、Write、Edit、Bash
- **操作参数**：如文件路径、命令内容
- **执行结果**：操作的输出或错误信息

```
[Read] src/auth/jwt.ts
  → 文件内容: ... (显示文件内容)

[Bash] npm test -- --filter auth
  → 测试结果: 5 passed, 1 failed

[Edit] src/auth/jwt.ts
  → 替换: line 42-45
  → 变更预览: ...
```

**Subagent 追踪**：当 Claude 使用 Task Tool 创建子任务时，可以展开查看每个子任务的独立执行日志 [S036]。

### 14.2 日志分析

**日志文件位置** [S034, S036]：

```bash
# Claude Code 日志目录
ls ~/.claude/logs/

# 查看最新日志
tail -f ~/.claude/logs/latest.log

# 搜索特定错误
grep "ERROR" ~/.claude/logs/*.log
```

**常见错误信息及含义**：

| 错误信息 | 含义 | 解决方向 |
|---------|------|---------|
| `Authentication failed` | API 认证失败 | 检查 API Key 或重新登录 |
| `Context window exceeded` | 上下文超限 | 使用 /compact 或 /clear |
| `Tool execution timeout` | 工具执行超时 | 检查网络，简化操作 |
| `MCP server disconnected` | MCP 服务断开 | 重启 MCP Server |
| `Rate limit exceeded` | API 限流 | 等待后重试，降低请求频率 |

### 14.3 错误定位

**API 错误定位** [S033, S034]：

```bash
# 开启详细日志模式
export CLAUDE_DEBUG=1
claude

# 测试 API 连通性
curl -H "x-api-key: $ANTHROPIC_API_KEY" \
  https://api.anthropic.com/v1/messages \
  -d '{"model":"claude-sonnet-4-20250514","max_tokens":10,"messages":[{"role":"user","content":"test"}]}'
```

**配置错误定位** [S032, S033]：

```bash
# 检查 CLAUDE.md 语法
cat CLAUDE.md  # 确认无格式错误

# 检查 MCP 配置
cat .claude/mcp.json | python3 -m json.tool  # 验证 JSON 格式

# 检查 settings.json
cat ~/.claude/settings.json | python3 -m json.tool
```

### 14.4 问题反馈渠道

当遇到无法自行解决的问题时 [S011, S001]：

1. **GitHub Issues**：在 Claude Code 的 GitHub 仓库提交 Issue，附带错误日志和复现步骤
2. **官方文档**：查阅 https://code.claude.com/docs 的 Troubleshooting 章节
3. **社区论坛**：在 Claude 社区发帖求助，附带环境信息和错误截图

### 14.5 系统化排查流程（5 步法）

当遇到问题时，建议按以下流程系统化排查 [S033, S034]：

```
步骤 1: 检查系统环境
├── node --version (是否 >= 18)
├── npm --version
├── which claude (路径是否正确)
└── claude --version (版本是否最新)

步骤 2: 验证 API 凭证
├── echo $ANTHROPIC_API_KEY (是否设置)
├── API Key 格式检查 (sk-ant-...)
└── 网络连通性测试 (curl api.anthropic.com)

步骤 3: 查看配置文件
├── cat CLAUDE.md (项目级)
├── cat ~/.claude/CLAUDE.md (用户级)
├── cat .claude/mcp.json (MCP 配置)
└── cat ~/.claude/settings.json (全局设置)

步骤 4: 重置配置（必要时）
├── mv ~/.claude ~/.claude.backup
├── claude (重新初始化)
└── 逐步恢复配置，定位问题项

步骤 5: 查看日志输出
├── CLAUDE_DEBUG=1 claude (开启调试)
├── tail -f ~/.claude/logs/latest.log
└── 收集错误信息，提交反馈
```

---

# 附录

## A. 实战案例精选

### 案例一：1 小时搭建 Web 应用

使用 Claude Code 从零搭建一个全栈 Web 应用 [S024]：

```bash
# 第1步：项目初始化（5分钟）
claude
> 创建一个 React + TypeScript + Vite 项目
> 添加 Tailwind CSS 和 shadcn/ui
> 配置 ESLint 和 Prettier

# 第2步：数据库设计（10分钟）
> 设计一个待办事项应用的数据库表结构
> 使用 Prisma 作为 ORM，生成迁移文件

# 第3步：后端 API（15分钟）
> 实现 RESTful API：CRUD 操作
> 添加用户认证（JWT）
> 编写 API 测试

# 第4步：前端页面（20分钟）
> 实现登录页、注册页、待办事项列表页
> 使用 React Query 管理 API 状态
> 添加响应式布局

# 第5步：测试与部署（10分钟）
> 运行所有测试确认通过
> 生成 Docker 配置文件
> 创建 GitHub Actions CI/CD 配置
```

### 案例二：大型项目重构

使用探索-规划-编码流程进行大规模重构 [S026]：

```bash
# 探索阶段
> [Plan 模式] 分析这个 18000+ 行的 monolith 文件
> 识别可以拆分的模块和它们的依赖关系

# 规划阶段
> 制定拆分计划，确保每个步骤都有对应的测试验证
> 列出需要修改的文件清单和修改顺序

# 编码阶段（分步执行）
> 第1步：提取 UserModule，保持所有测试通过
> 第2步：提取 PaymentModule，处理跨模块依赖
> ...
```

### 案例三：多 Claude 协作

使用三终端协作模式开发新功能 [S020, S039]：

```bash
# 终端 1：测试优先
claude
> 根据 PRD 文档为评论功能编写完整的测试套件

# 终端 2：实现功能
claude
> 查看 tests/comments/ 下的测试用例，实现评论功能使所有测试通过

# 终端 3：代码审查
claude
> 持续监控 src/comments/ 和 tests/comments/ 的变更
> 进行安全审查和代码质量检查
```

---

## B. 常用配置模板

### CLAUDE.md 最小模板

```markdown
# 项目名称

## 技术栈
- 前端: React + TypeScript
- 后端: Node.js + Express
- 数据库: PostgreSQL

## 编码规范
- TypeScript strict 模式
- Conventional Commits
- Prettier 格式化

## 常用命令
- `npm run dev` - 启动开发服务器
- `npm run test` - 运行测试
- `npm run build` - 构建生产版本
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
        "command": "npx prettier --write $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### MCP 配置模板

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${BRAVE_API_KEY}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-playwright"]
    }
  }
}
```

---

## C. 资源链接

### 官方资源
- **Claude Code 官方文档**: https://code.claude.com/docs
- **Anthropic 官方博客**: https://www.claude.com/blog
- **Claude Code GitHub**: https://github.com/anthropics/claude-code
- **Skills 开发指南**: https://platform.claude.com/docs/en/agents-and-tools/agent-skills
- **MCP 协议规范**: https://modelcontextprotocol.io

### 社区资源
- **Claude 中文社区**: https://claudecn.com
- **Claude Code 教程中心**: https://claudecode101.com/zh/tutorial
- **Claude Code Hub**: https://claudecodehub.github.io
- **Skills Fan**: https://www.skills.fan

### 学习资源
- **从入门到精通**: https://www.cnblogs.com/leadingcode/p/19084161
- **完全指南**: https://www.cnblogs.com/knqiufan/p/19449849
- **视频教程**: https://www.bilibili.com/video/BV1KzYQzWEPx/

---

## D. 参考文献

### A 级来源（官方文档）
- [S001] Claude Code 官方快速入门. Anthropic. https://code.claude.com/docs/zh-CN/quickstart
- [S002] Claude Code 概览. Anthropic. https://code.claude.com/docs/zh-CN/overview
- [S003] Claude Code 最佳实践. Anthropic. https://code.claude.com/docs/en/best-practices
- [S004] 内存管理官方文档. Anthropic. https://code.claude.com/docs/zh-CN/memory
- [S005] 官方故障排除文档. Anthropic. https://code.claude.com/docs/zh-TW/troubleshooting
- [S006] Skills 最佳实践. Anthropic Platform. https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- [S007] Skills 完整指南 PDF. Anthropic. https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf
- [S008] Skills 和 MCP 扩展博客. Anthropic. https://www.claude.com/blog/extending-claude-capabilities-with-skills-mcp-servers

### B 级来源（权威社区）
- [S009] Claude Code 从入门到精通. 博客园. https://www.cnblogs.com/leadingcode/p/19084161
- [S010] 最全 Claude Code 使用指南. 知乎. https://zhuanlan.zhihu.com/p/1954938233126381432
- [S011] Claude Code 完整使用指南. GitHub. https://github.com/Joseph19820124/claude-code-guide
- [S012] ClaudeCode 教程中心. https://claudecode101.com/zh/tutorial
- [S013] Claude 中文入门指南. https://claudecn.com/docs/claude-code/getting-started/
- [S014] Skills 和 MCP Servers 能力. Medium. https://medium.com/@moelkholy1995/claude-code-capabilities-with-skills-and-mcp-servers-2b5e87ea8e0d
- [S015] MCP Server 完整配置指南. BrainGrid. https://www.braingrid.ai/blog/claude-code-mcp
- [S016] MCP 上下文优化. Scott Spence. https://scottspence.com/posts/optimising-mcp-server-context-usage-in-claude-code
- [S017] Skills vs MCP vs Agents. Colin McNamara. https://colinmcnamara.com/blog/understanding-skills-agents-and-mcp-in-claude-code
- [S018] MCP 工具配置指南. Clockwise. https://www.getclockwise.com/blog/claude-code-mcp-tools-integration
- [S019] Claude Code 中高级技巧. 天盘博客. https://tianpan.co/zh/blog/2025-08-21-claude-code-tips
- [S020] 多 Claude 协作工作流. Claude 中文. https://claudecn.com/docs/claude-code/workflows/multi-claude/
- [S021] Skills 实用指南. 腾讯新闻. https://news.qq.com/rain/a/20260116A01DJS00
- [S023] AI编程实践与协作. 得物技术/腾讯云. https://cloud.tencent.com/developer/article/2624381
- [S026] Claude Code 完全指南. 博客园. https://www.cnblogs.com/knqiufan/p/19449849
- [S027] CLAUDE.md 8条黄金法则. DAMO开发者. https://damodev.csdn.net/696c8c73a16c6648a98333c7.html
- [S028] 从0到1搭建项目环境. 腾讯云. https://cloud.tencent.com/developer/article/2596824
- [S029] Mintlify 文档配置. Mintlify. https://www.mintlify.com/docs/zh/guides/claude-code
- [S030] Claude 中文网内存管理. https://www.claude-cn.org/claude-code-docs-zh/configuration/memory
- [S031] Claude Code Hub 内存. https://claudecodehub.github.io/memory.html
- [S032] Skills Fan FAQ. https://www.skills.fan/docs/troubleshooting/faq
- [S033] 完整故障排查指南. Hrefgo. https://hrefgo.com/zh/blog/claude-code-faq-troubleshooting-support
- [S034] 架构层面故障排除. 腾讯云. https://cloud.tencent.com/developer/article/2555179
- [S035] Claude Code Pro FAQ. https://claudecode.pro/zh/docs/faq
- [S039] 多Claude协作教程. ClaudeCode101. https://claudecode101.com/zh/tutorial/advanced/multi-claude

### C 级来源（社区分享）
- [S022] 高阶用法与骚技巧. DEV Community. https://dev.to/dragon72463399/claude-code-gao-jie-yong-fa-sao-ji-qiao-2bf7
- [S024] 1小时搞定Web应用. 腾讯云. https://cloud.tencent.com/developer/news/2853975
- [S025] B站实战教程. Bilibili. https://www.bilibili.com/video/BV1KzYQzWEPx/
- [S036] 傻瓜式调试技巧. Medium. https://medium.com/@marklaik/day-12-傻瓜式調適-claude-code產生的bugs-b6f4777c452e
- [S037] 高级技巧效率翻倍. 53AI. https://www.53ai.com/news/tishicijiqiao/2025092397623.html
- [S038] 李锋镝高级技巧. https://www.lifengdi.com/ren-gong-zhi-neng/4619
- [S040] 菜鸟教程入门. http://www.runoob.com/ai-agent/claude-code.html

---

> **版权声明**: 本报告基于公开资料整理，仅供学习参考。所有引用来源的版权归原作者所有。
>
> **生成时间**: 2026-02-10 | **版本**: Version 2 (架构思维与设计模式增强版)
