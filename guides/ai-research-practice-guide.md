# AI 调研实践指南：从零开始用 Claude Code 做技术调研

> 写给 AI 小白的实操手册
> 日期：2026-01-30

---

## 你的目标

搞清楚 AI 领域的技术全貌：有哪些技术栈、核心概念、关键名词、主流工具。

## 你手上有什么武器

Claude Code 不只是一个聊天机器人，它是一个**工具生态系统**。你可以把它理解为一个"瑞士军刀"，刀片（功能）分好几层：

```
┌─────────────────────────────────────────────┐
│              你（提问 / 下指令）               │
├─────────────────────────────────────────────┤
│  第 1 层：直接对话                            │
│  → 直接问 Claude，它用自身知识回答             │
├─────────────────────────────────────────────┤
│  第 2 层：Skills（技能）                      │
│  → 预写好的 SOP，让 Claude 按标准流程做事      │
├─────────────────────────────────────────────┤
│  第 3 层：Agent（子代理）                     │
│  → 派出"分身"去做专门的子任务                  │
├─────────────────────────────────────────────┤
│  第 4 层：MCP Server（外部接口）              │
│  → 连接外部工具和数据源（搜索、数据库、API）    │
├─────────────────────────────────────────────┤
│  第 5 层：Hooks（钩子）                       │
│  → 在特定操作前后自动执行脚本                  │
└─────────────────────────────────────────────┘
```

---

## 第一部分：先理解这些概念

### 1. 概念速查表

| 概念 | 一句话解释 | 类比 |
|------|-----------|------|
| **Claude Code** | Anthropic 出品的命令行 AI 编程助手 | 一个住在终端里的高级程序员 |
| **Skill** | 预写好的指令文件（SKILL.md），让 AI 按流程办事 | 员工手册里的某个 SOP |
| **Slash Command** | 用 `/命令名` 手动触发一个 Skill | 快捷键 |
| **Agent（子代理）** | Claude 派出去执行子任务的"分身" | 你派出去跑腿的同事 |
| **MCP Server** | Model Context Protocol 服务器，给 AI 接外部能力 | 给手机装 App |
| **Plugin** | 打包好的一组 Skills + 配置，一键安装 | App Store 里的应用 |
| **Hook** | 在工具调用前后自动运行的脚本 | 门禁系统（进出自动触发） |
| **CLAUDE.md** | 项目级指令文件，Claude 启动时自动读取 | 公司规章制度 |
| **Context Window** | AI 一次能"看到"的信息量上限 | 你的桌面大小，放不下太多文件 |
| **Frontmatter** | 文件顶部 `---` 之间的 YAML 元数据 | 书的封面信息（书名、作者） |

### 2. 它们之间的关系

```
CLAUDE.md（全局规则，始终生效）
    │
    ├── Skills（按需加载的专业能力）
    │     ├── 用户通过 /命令 手动触发
    │     └── Claude 自动判断是否需要加载
    │
    ├── Agent（子代理，执行独立子任务）
    │     ├── Explore 代理 → 快速搜索代码库
    │     ├── Plan 代理 → 设计实施方案
    │     └── general-purpose 代理 → 通用任务
    │
    ├── MCP Server（外部数据接入）
    │     ├── Context7 → 查库/框架文档
    │     ├── Playwright → 控制浏览器
    │     ├── Exa → 搜索互联网
    │     └── DeepWiki → 查 GitHub 项目文档
    │
    └── Hooks（自动化钩子）
          └── 工具调用前后自动执行脚本
```

---

## 第二部分：实战 — 用这些工具做 AI 调研

### 场景 1：我想了解某个 AI 概念

**用法：直接对话**

最简单的方式，直接问：

```
你：什么是 RAG？用简单的话解释，给出它和其他技术的关系
你：LLM、SLM、Agent、MCP、RAG、Fine-tuning 这些概念之间是什么关系？画一张关系图
```

**适合**：快速了解概念、术语解释、概念间的关系梳理。
**局限**：Claude 的知识有截止日期（2025 年 5 月），最新信息可能不全。

---

### 场景 2：我想查最新的 AI 技术动态

**用法：MCP — 联网搜索**

Claude Code 已经连接了多个搜索工具，你可以直接说：

```
你：帮我搜索 2026 年最新的 AI Agent 框架有哪些，对比它们的优缺点
你：搜索一下目前主流的 RAG 实现方案有哪些
你：查一下 MCP 协议目前有哪些主流的 Server 实现
```

Claude 会自动选择合适的搜索工具：
- **WebSearch / Exa** → 搜索互联网
- **DeepWiki** → 查 GitHub 项目的文档
- **Context7** → 查编程库/框架的官方文档

**适合**：获取最新信息、对比技术方案、了解行业动态。

---

### 场景 3：我想深入研究某个开源项目

**用法：MCP — DeepWiki + Exa**

```
你：帮我调研 langchain 这个项目，它是做什么的，核心概念有哪些，怎么用
你：查一下 dify 和 langchain 的区别，各自适合什么场景
你：帮我看看 vercel/ai 这个项目的文档，总结它的核心功能
```

背后发生的事：
1. Claude 调用 DeepWiki 拉取项目文档
2. 调用 Exa/WebSearch 搜索社区评价和教程
3. 综合分析后给你结构化的总结

---

### 场景 4：我想系统性地做一次调研并输出报告

**用法：Skill + Agent + MCP 组合拳**

这是最强大的用法。你可以让 Claude 帮你做一次完整的技术调研：

```
你：帮我做一份 AI 技术栈全景调研报告，包含以下内容：
1. AI 领域的主要分支（NLP、CV、多模态等）
2. 每个分支的主流框架和工具
3. 关键概念和术语表
4. 2025-2026 年的重要趋势
5. 给初学者的学习路线图

把结果写成一个 markdown 文件保存到当前目录
```

Claude 会：
1. 用自身知识构建基础框架
2. 通过 MCP 搜索工具补充最新信息
3. 派出 Agent 子代理执行子任务（如果需要）
4. 把结果写入文件

---

### 场景 5：我想快速学习一个新技术

**用法：利用现有 Skill 生态**

参考你的 skills guide，有专门做这件事的工具：

| 工具 | 用途 | 怎么用 |
|------|------|--------|
| [learn-faster-kit](https://github.com/cheukyin175/learn-faster-kit) | 基于 FASTER 方法论的学习框架 | 安装后按引导学习 |
| [obra/superpowers](https://github.com/obra/superpowers) 的 `/brainstorm` | 头脑风暴，探索技术方案 | `/brainstorm` 然后描述你想探索的话题 |
| [context-engineering-kit](https://github.com/NeoLabHQ/context-engineering-kit) | 高效利用上下文的技巧 | 提升你和 AI 协作的效率 |

安装方式：
```bash
# 安装 superpowers（推荐，最实用）
# 在 Claude Code 中输入：
/plugin marketplace add obra/superpowers-marketplace
```

---

## 第三部分：从零搭建你的 AI 调研工作台

### 第一步：确认你的 Claude Code 已有能力

你当前已经有这些 MCP 工具可用（不需要额外安装）：

| 工具 | 能力 | 调研用途 |
|------|------|----------|
| **WebSearch** | 搜索互联网 | 查最新资讯、论文、博客 |
| **Exa** | AI 增强搜索 | 深度搜索技术文章、代码示例 |
| **Context7** | 查编程库文档 | 了解某个框架的 API 和用法 |
| **DeepWiki** | 查 GitHub 项目文档 | 深入了解开源项目 |
| **Playwright** | 控制浏览器 | 自动化浏览网页、截图 |
| **WebFetch** | 抓取网页内容 | 读取特定网页并提取信息 |

**验证方式**：直接让 Claude 搜索试试：
```
你：帮我搜索 "AI Agent framework 2026" 的最新信息
```
如果返回了搜索结果，说明可用。

### 第二步：创建你的调研 Skill（可选但推荐）

为你的调研任务创建一个专用 Skill，这样以后每次调研都能按统一流程执行：

```bash
mkdir -p .claude/skills/ai-research
```

然后创建 `.claude/skills/ai-research/SKILL.md`：

```markdown
---
name: ai-research
description: |
  Conduct structured AI technology research. Trigger when user asks
  to research AI topics, compare technologies, or create tech reports.
  Covers: tech stacks, concepts, tools, trends, learning paths.
allowed-tools:
  - Bash
  - Read
  - Write
---

# AI Technology Research

## When to use
- User asks to research an AI technology or concept
- User wants a comparison of AI tools/frameworks
- User needs a glossary or learning roadmap

## Research workflow

### Phase 1: Scope definition
1. Clarify the research topic and depth
2. List specific questions to answer
3. Define output format (report / glossary / comparison table / roadmap)

### Phase 2: Information gathering
1. Use WebSearch/Exa for latest articles, blogs, papers
2. Use DeepWiki for GitHub project documentation
3. Use Context7 for library-specific docs
4. Use internal knowledge for established concepts

### Phase 3: Analysis & synthesis
1. Cross-reference multiple sources
2. Identify consensus views vs. debates
3. Note knowledge cutoff limitations

### Phase 4: Output
1. Write structured markdown report
2. Include:
   - Executive summary (3-5 sentences)
   - Key concepts glossary
   - Technology comparison tables
   - Relationship diagrams (text-based)
   - Source links
3. Save to user's specified directory

## Output standards
- Use Chinese (Simplified) for all content
- Comments and code in English
- Include source URLs for verifiable claims
- Mark any information that may be outdated
```

### 第三步：安装推荐 Plugin（可选）

```
# 在 Claude Code 中输入：

# 官方 Skills（文档处理能力，调研报告可导出 Word/PPT）
/plugin marketplace add anthropics/skills

# Superpowers（头脑风暴 + 计划执行，调研利器）
/plugin marketplace add obra/superpowers-marketplace
```

---

## 第四部分：实操示例 — 你现在就可以做的事

### 示例 1：生成 AI 术语表

直接对 Claude 说：
```
帮我整理一份 AI 领域核心术语表，分类整理，每个术语给出：
1. 英文名
2. 中文名
3. 一句话解释（小白能懂）
4. 和其他术语的关系

分类至少包含：基础概念、模型类型、训练方法、应用框架、部署运维
搜索最新资料补充 2025-2026 年新出现的重要术语
写入 ai-glossary.md 文件
```

### 示例 2：对比主流 AI 框架

```
帮我调研并对比以下 AI 开发框架：
- LangChain
- LlamaIndex
- Dify
- Coze
- AutoGen

从以下维度对比：
1. 定位和核心功能
2. 适用场景
3. 学习难度
4. 社区活跃度
5. 2026 年最新动态

搜索最新信息，结果写入 ai-frameworks-comparison.md
```

### 示例 3：画 AI 技术全景图

```
帮我梳理 AI 技术全景图，用文字版关系图展示：
1. AI 的主要分支（NLP、CV、多模态、Agent 等）
2. 每个分支下的核心技术和主流工具
3. 各分支之间的交叉和融合趋势
4. 标注哪些是 2025-2026 年的热点

搜索最新信息补充，写入 ai-landscape.md
```

### 示例 4：制定学习路线

```
我是一个 AI 小白，有编程基础但没有 AI 经验。
帮我制定一个 AI 学习路线图：
1. 按阶段划分（入门 → 进阶 → 实战）
2. 每个阶段要学什么、用什么资源
3. 重点推荐免费资源
4. 标注 2026 年最值得关注的方向

搜索最新的学习资源和课程推荐，写入 ai-learning-roadmap.md
```

---

## 第五部分：进阶技巧

### 1. 让 Claude 帮你持续追踪

创建一个追踪用的 Skill，定期执行：
```
帮我搜索本周 AI 领域的重要新闻和发布，
重点关注：新模型发布、新框架、重要论文、行业动态
写入 weekly-ai-digest-{日期}.md
```

### 2. 用 Playwright 自动化浏览

如果某个网站需要浏览器访问：
```
用浏览器打开 https://huggingface.co/models
帮我看看目前最热门的模型有哪些，截图并总结
```

### 3. 组合技：调研 → 报告 → 导出

如果安装了官方 Skills（docx/pptx）：
```
1. 先做调研，生成 markdown 报告
2. 用 /docx 把报告转成 Word 文档
3. 用 /pptx 把关键内容做成 PPT
```

### 4. 用 CLAUDE.md 设定调研偏好

在你的项目目录创建 `.claude/CLAUDE.md`，写入你的调研偏好：

```markdown
# Research Preferences

- All output in Chinese (Simplified)
- Always search for latest information before answering
- Include source URLs for all claims
- Use comparison tables for tool/framework evaluations
- Mark information freshness (knowledge cutoff vs. live search)
```

这样每次 Claude 启动都会自动遵循这些规则。

---

## 总结：你的工具箱一览

| 你想做的事 | 用什么 | 怎么用 |
|-----------|--------|--------|
| 快速了解一个概念 | 直接对话 | 直接问 Claude |
| 查最新信息 | MCP（WebSearch/Exa） | 让 Claude 搜索 |
| 查开源项目 | MCP（DeepWiki） | 让 Claude 查 GitHub 文档 |
| 查库/框架文档 | MCP（Context7） | 让 Claude 查 API 文档 |
| 按流程做调研 | Skill | 创建调研 Skill 或安装现有的 |
| 批量处理子任务 | Agent | Claude 自动派子代理 |
| 导出报告 | Plugin（官方 docx/pptx） | `/docx` 或 `/pptx` |
| 自动化浏览网页 | MCP（Playwright） | 让 Claude 操作浏览器 |
| 统一调研规范 | CLAUDE.md | 写入项目级配置 |
