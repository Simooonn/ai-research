# Agent Skills 与 Claude Code Skills 完全指南

> 调研日期：2026-01-29
> 适合读者：零基础入门

---

## 第一部分：Agent Skills — 开放标准

### 一、Agent Skills 是什么？

Agent Skills 是一个**开放标准格式**，用于给 AI 代理（Agent）扩展能力。

用一个类比：
- AI 代理 = 一个聪明但没有专业背景的新员工
- Skill = 你给这个员工写的「标准操作手册（SOP）」
- 没有 Skill 时，你每次都要口头交代一遍怎么做；有了 Skill，员工自己翻手册就行

**核心思想**：不是把所有知识一次性塞给 AI（那样会撑爆上下文窗口），而是让 AI **按需加载**——需要什么能力，就去「翻」对应的手册。

### 二、为什么需要 Agent Skills？

传统做法的问题：

❌ 写一个巨长的 system prompt，把所有规则和流程都塞进去
   → 上下文窗口被占满，AI 反而变笨
   → 大部分信息当前任务根本用不到
Agent Skills 的解决方式：

✅ 把知识拆成一个个独立的 Skill 文件
   → AI 启动时只读取每个 Skill 的名称和描述（很短）
   → 遇到匹配的任务时，才加载完整指令
   → 上下文窗口干净高效
这叫做 **渐进式披露（Progressive Disclosure）**：

1. **发现阶段**：AI 启动时，只加载所有 Skill 的 name 和 `description`（几行文字）
2. **激活阶段**：当用户的任务匹配某个 Skill 的描述时，AI 读取完整的 SKILL.md 指令
3. **执行阶段**：AI 按照指令操作，必要时加载引用的文件或执行附带的脚本

### 三、一个 Skill 长什么样？

每个 Skill 是一个**文件夹**，最少包含一个 SKILL.md 文件：

my-skill/
├── SKILL.md        # 【必须】指令 + 元数据
├── scripts/        # 【可选】可执行脚本
├── references/     # 【可选】参考文档
└── assets/         # 【可选】模板、资源文件
SKILL.md 文件由两部分组成：

---
name: pdf-processing
description: Extract text and tables from PDF files, fill forms, merge documents.
---

# PDF Processing

## When to use this skill
Use this skill when the user needs to work with PDF files...

## How to extract text
1. Use pdfplumber for text extraction...

## How to fill forms
...
- 上半部分（`---` 之间）= **YAML frontmatter**，是元数据（名称、描述等）
- 下半部分 = **Markdown 正文**，是给 AI 看的具体指令

### 四、标准规范要点

根据 agentskills.io 的规范：

| 字段 | 是否必须 | 限制 | 说明 |
|------|----------|------|------|
| name | 是 | 最多 64 字符，小写字母+数字+连字符 | 技能标识符 |
| description | 是 | 最多 1024 字符 | 描述做什么、何时使用 |
| license | 否 | - | 开源许可证 |
| compatibility | 否 | 最多 500 字符 | 环境要求（哪个产品、系统依赖等） |
| metadata | 否 | - | 任意键值对（作者、版本等） |
| allowed-tools | 否 | 空格分隔列表 | 技能可使用的工具（实验性） |

### 五、谁在支持这个标准？

Agent Skills 最初由 Anthropic 开发，后作为开放标准发布：

| 工具 | 支持情况 |
|------|----------|
| Claude Code | 完整支持 + 私有扩展功能 |
| Cursor | 支持 |
| Codex (OpenAI) | 支持 |
| 其他 AI 编码工具 | 逐步跟进中 |

这意味着：你写一个 SKILL.md`，放在项目里，理论上多个 AI 工具都能识别它。就像 .editorconfig` 对编辑器一样——一份配置，多工具通用。

### 六、核心价值总结

| 价值 | 说明 |
|------|------|
| 可移植 | 同一个 Skill 跨工具使用，不绑定某个平台 |
| 可版本控制 | 就是普通文件夹，直接用 Git 管理 |
| 可共享 | 团队成员、社区都可以复用 |
| 按需加载 | 不浪费上下文窗口 |
| 自文档化 | Markdown 格式，人和 AI 都能读 |

---

## 第二部分：Claude Code 中的 Skills

Claude Code 是目前对 Agent Skills 标准支持**最完整**的工具，并且在标准基础上增加了很多**私有扩展功能**。

### 七、Claude Code Skills 与标准的关系

Agent Skills 开放标准（基础）
  ├── name, description        ← 所有兼容工具都支持
  ├── scripts/, references/    ← 所有兼容工具都支持
  │
  └── Claude Code 私有扩展（增强）
      ├── 斜杠命令调用 /skill-name
      ├── 自动触发（AI 自主判断是否加载）
      ├── 调用控制（user-invocable / model-invocable）
      ├── 子代理执行（agent 字段）
      ├── 动态上下文注入（context 字段）
      └── 与旧版 slash commands 向后兼容
### 八、历史背景：斜杠命令合并进 Skills

Claude Code 早期有两套扩展机制：

| 旧机制 | 路径 | 特点 |
|--------|------|------|
| Slash Commands | .claude/commands/review.md | 单个 md 文件，简单 |
| Skills | .claude/skills/review/SKILL.md | 文件夹，支持 frontmatter 和辅助文件 |

**2026 年 1 月（v2.1.3）**，Anthropic 将两者统一为 Skills 系统：

- 旧的 .claude/commands/ 文件**继续有效**，无需迁移
- 两种写法都会注册为 /review 命令
- Skills 额外支持：文件夹结构、frontmatter 元数据、自动触发等

### 九、Skills 的存放位置

| 级别 | 路径 | 作用范围 |
|------|------|----------|
| 个人全局 | ~/.claude/skills/<skill-name>/SKILL.md | 你所有项目都可用 |
| 项目级 | <项目根目录>/.claude/skills/<skill-name>/SKILL.md | 仅当前项目，可 git 提交共享给团队 |
| 嵌套发现 | 项目中任意 skills/ 目录下的 SKILL.md | 自动发现，适合 monorepo |

### 九（补充）、Skills 优先级规则

当同名 Skill 存在于多个层级时，**优先级从高到低**：

| 优先级 | 层级 | 路径 | 规则说明 |
|--------|------|------|----------|
| 🥇 最高 | 嵌套（最近目录） | 当前编辑文件所在子目录的 .claude/skills/ | **就近原则**：最靠近当前上下文的优先 |
| 🥈 次高 | 项目级 | <项目根>/.claude/skills/<skill-name>/SKILL.md | 项目维度覆盖全局 |
| 🥉 最低 | 全局级 | ~/.claude/skills/<skill-name>/SKILL.md | 用户维度，兜底 |

核心原则：越具体（离当前上下文越近）的 Skill 优先级越高。 这与 CSS 就近优先、Git 配置继承（local > global > system）的设计思路一致。

**实际场景示例**（monorepo）：
my-monorepo/
├── .claude/skills/lint/SKILL.md          # 项目级 lint skill
├── packages/
│   └── frontend/
│       └── .claude/skills/lint/SKILL.md  # 前端专用 lint skill（优先级更高）
│   └── backend/
│       └── .claude/skills/lint/SKILL.md  # 后端专用 lint skill（优先级更高）
当你编辑 packages/frontend/src/App.tsx 时，Claude 会加载 `packages/frontend/.claude/skills/lint/SKILL.md`，而不是根目录的。

> ⚠️ **已知问题**：子代理（subagent）可能会加载全局目录的 skill 而不是项目目录的 skill（[GitHub Issue #10061](https://github.com/anthropics/claude-code/issues/10061)）。如遇到此情况属已知 bug，可关注 issue 跟进修复。

---

### 十、如何创建一个 Skill（实操步骤）

#### 第一步：创建目录和文件

# 项目级（推荐，团队共享）
mkdir -p .claude/skills/explain-code
touch .claude/skills/explain-code/SKILL.md

# 或者个人全局（只有自己用）
mkdir -p ~/.claude/skills/explain-code
touch ~/.claude/skills/explain-code/SKILL.md
#### 第二步：编写 SKILL.md

---
name: explain-code
description: |
  Explains code using visual diagrams and analogies.
  Trigger when user asks "how does X work" or "explain this code".
---

# Code Explainer

When explaining code, follow these steps:

1. Start with a one-sentence summary of what the code does
2. Break down the logic into numbered steps
3. Use an analogy to explain the core concept
4. Highlight any edge cases or gotchas
#### 第三步：使用

# 手动调用
> /explain-code

# 或者直接提问，Claude 会自动匹配
> 这段代码是怎么工作的？
### 十一、Claude Code 扩展的 Frontmatter 字段

除了标准字段（name、description、license、metadata、allowed-tools），Claude Code 额外支持：

---
name: my-skill
description: What this skill does.

# --- Claude Code 扩展字段 ---

# 调用控制
user-invocable: true              # 用户能否通过 /my-skill 调用（默认 true）
model-invocable: true             # Claude 能否自动调用（默认 true）
disable-model-invocation: false   # 设为 true 则禁止 Claude 自动调用

# 子代理执行
agent: Explore    # 在哪个子代理中运行，可选：Explore / Plan / general-purpose / 自定义代理

# 动态上下文注入
context:
  - type: command
    command: git log --oneline -10
    description: "Recent git history"

# 工具权限
allowed-tools:
  - Bash
  - Read
  - Write
---
| 扩展字段 | 说明 |
|----------|------|
| user-invocable | 用户能否手动调用（默认 true） |
| model-invocable | Claude 能否自主调用（默认 true） |
| disable-model-invocation | 设为 true 禁止 AI 自动调用，只能用户手动 / 调用 |
| agent | 指定在子代理中运行（隔离上下文，结果汇总返回） |
| context | 运行前自动执行命令，把结果注入上下文 |
| allowed-tools | 技能激活时授权使用的工具列表 |

### 十二、两种触发方式

#### 1. 手动调用（斜杠命令）

/commit
/commit -m "feat: add login page"
/review --security
#### 2. 自动触发

Claude 启动时会读取所有 Skill 的 `description`。当用户的任务匹配时，Claude 自动加载并使用。

> **注意**：所有 Skill 的描述总量有字符预算限制（默认 15,000 字符）。Skill 多的话描述要精炼。

### 十三、实用示例

#### 示例 1：Git Commit 助手

---
name: commit
description: |
  Helps create well-formatted git commit messages following
  conventional commits format. Trigger when user wants to commit code.
allowed-tools:
  - Bash
---

# Git Commit Helper

1. Run `git diff --staged` to see staged changes
2. Analyze the changes and categorize (feat/fix/refactor/docs/test/chore)
3. Write a commit message:
   - Title: `type(scope): brief description` (max 72 chars)
   - Body: explain WHY, not WHAT
4. Ask user to confirm before committing
#### 示例 2：代码审查

---
name: review
description: |
  Reviews code for bugs, security issues, and best practices.
  Trigger when user asks for code review or says "review this".
---

# Code Review Checklist

Review the code for:

1. **Bug risks**: null checks, off-by-one errors, race conditions
2. **Security**: SQL injection, XSS, hardcoded secrets
3. **Performance**: N+1 queries, memory leaks
4. **Readability**: naming, function length
5. **Tests**: are edge cases covered?

Output as structured review with severity levels (critical/warning/info).
#### 示例 3：使用子代理的研究技能

---
name: research
description: Research a topic thoroughly using web search and codebase exploration.
agent: Explore
---

# Research Skill

1. Search the codebase for relevant files
2. Read and analyze key implementations
3. Summarize findings in a structured format
#### 示例 4：带辅助文件的技能
.claude/skills/api-design/
├── SKILL.md                   # 主技能文件
├── openapi-template.yaml      # API 模板
├── naming-conventions.md      # 命名规范
└── examples/
    └── user-api.yaml          # 示例
### 十四、字符串替换变量

在 Skill 正文中可使用：

| 变量 | 含义 |
|------|------|
| {{arguments}} | 用户传入的参数 |
| {{cwd}} | 当前工作目录 |
| {{date}} | 当前日期 |

### 十五、常见问题排查

| 问题 | 可能原因 | 解决方法 |
|------|----------|----------|
| Skill 不触发 | description 太模糊 | 让描述更具体，明确触发条件 |
| Skill 触发太频繁 | description 太宽泛 | 缩小范围，或设 disable-model-invocation: true |
| Claude 看不到 Skill | 描述总量超预算 | 精简描述，控制在 15,000 字符内 |
| 旧斜杠命令失效 | 文件位置不对 | 确认在 .claude/commands/ 或 .claude/skills/ 下 |

---

## 第三部分：快速上手

### 五分钟入门清单

1. **创建目录**：`mkdir -p .claude/skills/my-skill`
2. **编写 SKILL.md**：写好 frontmatter（name + description）+ 正文指令
3. **测试**：在 Claude Code 中输入 /my-skill
4. **调优**：根据自动触发效果调整 description
5. **分享**：提交到 Git，团队成员自动获得

### 去哪里找现成的 Skills？

| 资源入口 | 类型 | ⭐ Stars | 说明 |
|----------|------|----------|------|
| [anthropics/skills](https://github.com/anthropics/skills) | 官方仓库 | ~57.9k | 官方维护的高质量 Skill 集合，含文档类、设计类、开发类 Skill |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 社区 Awesome 列表 | ~21.6k | 最全的社区资源列表：Skills、Hooks、插件、工具、工作流等 |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 社区 Awesome 列表 | - | 专注 Skills 的精选列表，分类清晰 |
| [Agent Skills 标准官网](https://agentskills.io) | 规范文档 | - | 开放标准规范，跨工具通用 |
| [Claude Plugins 社区](https://claude-plugins.dev/) | 插件市场 | - | 社区维护的插件/技能浏览器，支持 CLI 安装 |

---

## 第四部分：各领域优质资源推荐

> 调研日期：2026-01-30。以下资源经过逐一核实，标注了**类型**（Skill / Plugin / Workflow / 工具 / Slash Command）、**GitHub Stars**（截至调研日期的近似值）、**质量评估**。

### 类型说明

在推荐前先厘清概念，避免混淆：

| 类型 | 定义 | 存放位置/调用方式 |
|------|------|-------------------|
| Skill | 包含 SKILL.md 的文件夹，遵循 Agent Skills 标准。Claude 可按需自动加载 | .claude/skills/<name>/SKILL.md |
| Plugin | Claude Code 插件，可包含多个 Skills + 配置。通过 /plugin install 安装 | 来自 marketplace 或本地目录 |
| Slash Command | 单个 .md 文件，手动通过 /command-name 调用 | .claude/commands/<name>.md |
| Workflow | 一组 Skills + Commands + CLAUDE.md + 子代理，构成完整开发流程 | 项目 .claude/ 目录下多个文件 |
| 工具（Tool） | 独立可执行程序/CLI，增强 Claude Code 功能 | 系统安装或 npm/pip 安装 |
| MCP Server | Model Context Protocol 服务器，为 Claude 提供外部数据/API 接入 | 独立进程，Claude Code 配置连接 |
| CLAUDE.md | 项目/全局指令文件，始终加载到上下文中 | .claude/CLAUDE.md |

### 十六、官方 Skills（来自 anthropics/skills）

> 安装方式：`/plugin marketplace add anthropics/skills` → 选择 document-skills 或 example-skills
| Skill 名称 | 类型 | 功能 | 适用场景 |
|------------|------|------|----------|
| [docx](https://github.com/anthropics/skills/tree/main/skills/docx) | Skill（官方） | 创建/编辑/分析 Word 文档，支持修订、批注、格式保留 | 需求文档、产品文档撰写 |
| [pdf](https://github.com/anthropics/skills/tree/main/skills/pdf) | Skill（官方） | PDF 文本提取、表格解析、合并/拆分、表单填写 | 文档处理、报告生成 |
| [pptx](https://github.com/anthropics/skills/tree/main/skills/pptx) | Skill（官方） | 创建/编辑 PPT，支持布局、模板、图表、自动生成幻灯片 | 产品发布、方案演示 |
| [xlsx](https://github.com/anthropics/skills/tree/main/skills/xlsx) | Skill（官方） | 创建/编辑 Excel，支持公式、格式化、数据分析、可视化 | 数据分析、报表制作 |
| [frontend-design](https://github.com/anthropics/skills/blob/main/skills/frontend-design) | Skill（官方） | 避免 AI "通病"审美，做出大胆设计决策。针对 React + Tailwind 优化 | UI 设计、前端开发 |
| [artifacts-builder](https://github.com/anthropics/skills/tree/main/skills/artifacts-builder) | Skill（官方） | 用 React + Tailwind + shadcn/ui 构建复杂 HTML 组件 | 前端组件开发 |
| [mcp-builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) | Skill（官方） | 指导构建高质量 MCP 服务器，集成外部 API | 后端开发、API 集成 |
| [webapp-testing](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) | Skill（官方） | 用 Playwright 测试本地 Web 应用的 UI 验证和调试 | QA 测试 |
| [canvas-design](https://github.com/anthropics/skills/tree/main/skills/canvas-design) | Skill（官方） | 设计精美视觉艺术（.png/.pdf），多种设计哲学 | UI 设计、视觉素材 |
| [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) | Skill（官方） | 交互式引导你创建新 Skill | 自定义 Skill 开发 |
| [internal-comms](https://github.com/anthropics/skills/tree/main/skills/internal-comms) | Skill（官方） | 撰写内部沟通文档：状态报告、Newsletter、FAQ | 产品文档、团队沟通 |
| [brand-guidelines](https://github.com/anthropics/skills/tree/main/skills/brand-guidelines) | Skill（官方） | 应用品牌色彩和排版规范 | 品牌设计、UI 一致性 |

### 十七、社区精选资源（按领域分类）

#### 📊 综合资源平台

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [obra/superpowers](https://github.com/obra/superpowers) | Plugin（含 20+ Skills） | ~38.5k | ⭐⭐⭐⭐⭐ | 实战检验的核心技能库：TDD、调试、协作、代码审查。含 /brainstorm`、`/write-plan`、`/execute-plan 命令。作者 Jesse Vincent 是知名开源贡献者。安装：`/plugin marketplace add obra/superpowers-marketplace` |
| [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) | Plugin（Skills + Commands + Agents + Hooks + MCP） | ~31.7k | ⭐⭐⭐⭐⭐ | Anthropic 黑客松获奖作品。覆盖几乎所有 Claude Code 特性（agents/skills/hooks/commands/rules/MCPs），单独资源质量也很高 |
| [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) | Plugin（Skills + Agents + Commands） | - | ⭐⭐⭐⭐ | 务实的工程实践框架：将错误和教训转化为未来改进。文档清晰，设计合理 |

#### 🔍 技术调研 & 快速学习

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [cheukyin175/learn-faster-kit](https://github.com/cheukyin175/learn-faster-kit) | Workflow（Agents + Commands） | - | ⭐⭐⭐⭐ | 基于 FASTER 自学方法论的教育框架，含主动学习、间隔重复技术。适合快速学习新技术 |
| [NeoLabHQ/context-engineering-kit](https://github.com/NeoLabHQ/context-engineering-kit) | Workflow（Skills + Commands） | - | ⭐⭐⭐⭐ | 高级上下文工程技巧集合，最小化 token 开销同时最大化 AI 输出质量 |
| [obra/superpowers](https://github.com/obra/superpowers) → /brainstorm | Slash Command（Plugin 内） | 见上 | ⭐⭐⭐⭐⭐ | 头脑风暴命令，适合技术方案调研和探索 |

#### 📋 产品文档 & 需求文档

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [automazeio/ccpm](https://github.com/automazeio/ccpm) | Workflow（工具 + Agents） | - | ⭐⭐⭐⭐ | 全面的项目管理工作流，含专门的文档撰写代理，用 GitHub Issues + Git worktrees 并行执行 |
| [Helmi/claude-simone](https://github.com/Helmi/claude-simone) | Workflow（Commands + 文档系统） | - | ⭐⭐⭐⭐ | 项目管理工作流，不仅有命令，还有文档、指南、流程系统 |
| [Wirasm/claudecode-utils → /create-prp](https://github.com/Wirasm/claudecode-utils/blob/main/.claude/commands/create-prp.md) | Slash Command | - | ⭐⭐⭐ | 创建产品需求计划（PRP），遵循 PRP 方法论模板 |
| anthropics/skills → [internal-comms](https://github.com/anthropics/skills/tree/main/skills/internal-comms) | Skill（官方） | 见上 | ⭐⭐⭐⭐ | 状态报告、Newsletter、FAQ 等内部沟通文档 |

#### 🎨 UI 设计
| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| anthropics/skills → [frontend-design](https://github.com/anthropics/skills/blob/main/skills/frontend-design) | Skill（官方） | 见上 | ⭐⭐⭐⭐⭐ | **强烈推荐**。专门训练 Claude 避免 AI "slop"（千篇一律的审美），做出大胆、有个性的设计。React + Tailwind 效果最佳 |
| [OneRedOak/claude-code-workflows → design-review](https://github.com/OneRedOak/claude-code-workflows/tree/main/design-review) | Workflow（Agents + Commands） | - | ⭐⭐⭐⭐ | 自动化 UI/UX 设计评审工作流，覆盖响应式设计、无障碍性等多维度标准 |
| [alonw0/web-asset-generator](https://github.com/alonw0/web-asset-generator) | Skill | - | ⭐⭐⭐ | 生成 favicon、PWA 图标、OG 社交媒体 meta 图片，含 HTML meta 标签 |
| anthropics/skills → [canvas-design](https://github.com/anthropics/skills/tree/main/skills/canvas-design) | Skill（官方） | 见上 | ⭐⭐⭐⭐ | .png/.pdf 格式视觉设计 |

#### 💻 前端开发

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| anthropics/skills → [artifacts-builder](https://github.com/anthropics/skills/tree/main/skills/artifacts-builder) | Skill（官方） | 见上 | ⭐⭐⭐⭐ | React + Tailwind + shadcn/ui 组件构建 |
| [lackeyjb/playwright-skill](https://github.com/lackeyjb/playwright-skill) | Skill | - | ⭐⭐⭐ | 通用浏览器自动化，基于 Playwright |
| [chrisvoncsefalvay/claude-d3js-skill](https://github.com/chrisvoncsefalvay/claude-d3js-skill) | Skill | - | ⭐⭐⭐ | D3.js 数据可视化能力 |
| anthropics/skills → [webapp-testing](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) | Skill（官方） | 见上 | ⭐⭐⭐⭐ | Playwright 驱动的 Web 应用 UI 测试 |

#### ⚙️ 后端开发

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| anthropics/skills → [mcp-builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) | Skill（官方） | 见上 | ⭐⭐⭐⭐⭐ | 指导构建高质量 MCP 服务器以集成外部 API 和服务 |
| [fcakyon/claude-codex-settings](https://github.com/fcakyon/claude-codex-settings) | Plugin | - | ⭐⭐⭐⭐ | 覆盖 GitHub、Azure、MongoDB、Tavily、Playwright 等主流云平台和服务的插件集 |
| [tony/claude-code-riper-5](https://github.com/tony/claude-code-riper-5) | Workflow | - | ⭐⭐⭐⭐ | RIPER 五阶段工作流（Research→Innovate→Plan→Execute→Review），含分支感知记忆 |
| [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) | Skill 集合 | - | ⭐⭐⭐ | 科学计算技能集，含专业科学库和数据库操作 |

#### 🔧 运维工作

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [dagger/container-use](https://github.com/dagger/container-use) | 工具 | - | ⭐⭐⭐⭐⭐ | 为 AI 代理创建安全隔离的 Docker 开发环境，支持多代理并行工作 |
| [icanhasjonas/run-claude-docker](https://github.com/icanhasjonas/run-claude-docker) | 工具 | - | ⭐⭐⭐⭐ | Docker 隔离运行 Claude Code，保留 SSH agent/PGP/AWS 密钥等认证 |
| [diet103/claude-code-infrastructure-showcase](https://github.com/diet103/claude-code-infrastructure-showcase) | Workflow（Skills + Hooks） | - | ⭐⭐⭐⭐ | 创新性地用 Hooks 智能选择和激活合适的 Skill，文档清晰可复用 |
| [danielrosehill/Claude-Code-Linux-Desktop-Slash-Commands](https://github.com/danielrosehill/Claude-Code-Linux-Desktop-Slash-Commands) | Slash Commands 集合 | - | ⭐⭐⭐ | Linux 桌面/服务器运维专用命令集：硬件基准、文件系统、安全审计 |

#### 🧪 QA 测试

| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [trailofbits/skills](https://github.com/trailofbits/skills) | Skill 集合 | - | ⭐⭐⭐⭐⭐ | **顶级安全公司**出品。12+ 安全技能：CodeQL/Semgrep 静态分析、变体分析、代码审计、漏洞检测。专业级品质 |
| [nizos/tdd-guard](https://github.com/nizos/tdd-guard) | Hooks | - | ⭐⭐⭐⭐ | Hooks 驱动的 TDD 守卫，实时监控文件变更，阻止违反 TDD 原则的操作 |
| [zscott/pane → /tdd](https://github.com/zscott/pane/blob/main/.claude/commands/tdd.md) | Slash Command | - | ⭐⭐⭐⭐ | TDD 红-绿-重构工作流，集成 Git workflow 和 PR 创建 |
| [bartolli/claude-code-typescript-hooks](https://github.com/bartolli/claude-code-typescript-hooks) | Hooks | - | ⭐⭐⭐ | TypeScript 项目质量检查：编译 + ESLint 自动修复 + Prettier 格式化，<5ms 性能 |

#### 🏗️ 项目管理 & 工作流
| 名称 | 类型 | ⭐ Stars | 质量 | 说明 |
|------|------|----------|------|------|
| [automazeio/ccpm](https://github.com/automazeio/ccpm) | Workflow + 工具 | - | ⭐⭐⭐⭐ | Claude Code 项目管理系统，GitHub Issues + Git worktrees 并行代理执行 |
| [scopecraft/command](https://github.com/scopecraft/command) | Slash Commands 集合 | - | ⭐⭐⭐⭐ | 覆盖 SDLC 全流程的命令集：规划、实现、管理、发布 |
| [smtg-ai/claude-squad](https://github.com/smtg-ai/claude-squad) | 工具（编排器） | - | ⭐⭐⭐⭐ | 终端应用，管理多个 Claude Code 实例在独立工作区并行工作 |

### 十八、质量评估标准说明

上述资源的 质量评级 基于以下维度综合评估：

| 评级 | 含义 | 参考指标 |
|------|------|----------|
| ⭐⭐⭐⭐⭐ | 极致品质 | Stars > 10k / 官方出品 / 知名安全公司出品 / 社区公认标杆 |
| ⭐⭐⭐⭐ | 优秀可靠 | 文档清晰 / 活跃维护 / 设计合理 / 经社区验证 |
| ⭐⭐⭐ | 值得一试 | 功能明确 / 可用但可能不够成熟或文档较少 |

> **注意**：Stars 数量标记 - 表示未能精确获取（可能 <1k 或为新项目），不代表质量差。部分精品小众项目 Stars 不多但实用价值很高。

### 十九、快速安装指南

# 1. 安装 Anthropic 官方 Skills（最推荐，文档类 + 示例类）
/plugin marketplace add anthropics/skills
# 然后选择 document-skills 和/或 example-skills 安装

# 2. 安装 Superpowers 核心技能库（开发全流程）
/plugin marketplace add obra/superpowers-marketplace

# 3. 安装 Everything Claude Code（全能型）
/plugin install affaan-m/everything-claude-code

# 4. 单独安装某个 Skill（手动方式）
mkdir -p .claude/skills/my-skill
# 将 SKILL.md 文件放入该目录即可
---

## 参考资源

- [Agent Skills 开放标准](https://agentskills.io)
- [官方文档：Extend Claude with skills](https://code.claude.com/docs/en/skills)
- [Anthropic 官方 Skills 仓库](https://github.com/anthropics/skills) — ⭐ 57.9k
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code) — ⭐ 21.6k，最全的社区资源列表
- [Awesome Claude Skills](https://github.com/travisvn/awesome-claude-skills) — 专注 Skills 的精选列表
- [Claude Plugins 社区市场](https://claude-plugins.dev/) — 社区维护的插件浏览和 CLI 安装工具
- [Claude 官方博客：How to create Skills](https://www.claude.com/blog/how-to-create-skills-key-steps-limitations-and-examples)
- [Claude Code 功能扩展总览](https://code.claude.com/docs/en/features-overview)
- [Anthropic Opens Agent Skills Standard (Unite.AI)](https://www.unite.ai/anthropic-opens-agent-skills-standard-continuing-its-pattern-of-building-industry-infrastructure/)
- [Simon Willison 评价 Skills](https://simonwillison.net/2025/Oct/16/claude-skills/) — "可能比 MCP 更重要"