# Skill + Agent + MCP 组合拳：深度拆解

> 目标：彻底搞懂 Claude Code 最强大的用法 — 三层能力如何协作完成复杂任务

---

## 一、先搞清楚这三样东西各自干什么

### 1. MCP Server — "感官"

MCP（Model Context Protocol）是 Claude 连接外部世界的通道。

**没有 MCP 时**：Claude 只能用训练时学到的知识回答你，像一个闭卷考试的学生。

**有了 MCP 后**：Claude 可以实时搜索互联网、读取文档、操控浏览器，像一个开卷考试 + 能上网的学生。

```
你的问题
    │
    ▼
Claude 大脑（自身知识，截止 2025-05）
    │
    ├──→ WebSearch MCP ──→ 搜索互联网，拿到最新信息
    ├──→ Exa MCP ────────→ AI 增强搜索，找到高质量技术文章
    ├──→ Context7 MCP ───→ 查编程库的官方文档
    ├──→ DeepWiki MCP ───→ 查 GitHub 项目的结构化文档
    ├──→ Playwright MCP ─→ 打开浏览器，像人一样浏览网页
    └──→ WebFetch ───────→ 抓取指定网页的内容
```

**关键理解**：MCP 是被动工具。Claude 决定"我需要搜索一下"时，才会调用它。你也可以主动要求："用搜索查一下 xxx"。

### 2. Agent（子代理） — "分身"

Agent 是 Claude 派出去执行独立子任务的"分身"。

**没有 Agent 时**：Claude 一个人干所有事，上下文越来越长，越来越慢，容易遗忘前面的内容。

**有了 Agent 后**：Claude 可以把大任务拆成小块，每块派一个 Agent 去做，Agent 做完把结果汇总回来。

```
你：帮我调研 AI 技术全景
    │
    ▼
Claude（主代理，负责拆分任务 + 汇总结果）
    │
    ├── Agent 1（Explore 类型）
    │   任务：搜索 NLP 领域的最新框架
    │   工具：WebSearch, Exa, DeepWiki
    │   结果：返回一段结构化总结
    │
    ├── Agent 2（Explore 类型）
    │   任务：搜索计算机视觉领域的最新框架
    │   工具：WebSearch, Exa, DeepWiki
    │   结果：返回一段结构化总结
    │
    ├── Agent 3（general-purpose 类型）
    │   任务：整理 AI 术语表
    │   工具：WebSearch, Read, Write
    │   结果：返回术语表 markdown
    │
    └── Claude 主代理汇总所有结果 → 写入最终报告
```

**关键理解**：
- 每个 Agent 有**独立的上下文**，不会互相干扰
- Agent 执行完毕后，只把**结果**返回给主 Claude，不是把全部过程搬回来
- 这样主 Claude 的上下文始终保持干净

**Agent 的类型**：

| 类型 | 擅长什么 | 什么时候用 |
|------|---------|-----------|
| **Explore** | 快速搜索、浏览代码库、查找文件 | 需要搜索和阅读的任务 |
| **Plan** | 设计实施方案、分析架构 | 需要深度思考和规划的任务 |
| **general-purpose** | 通用任务，什么都能做 | 需要多步骤操作的复杂任务 |
| **Bash** | 执行命令行操作 | 需要运行脚本、安装工具 |

### 3. Skill — "剧本"

Skill 是你预先写好的"标准操作流程"。

**没有 Skill 时**：每次做调研，你都要把完整需求从头描述一遍。Claude 每次理解的可能不一样，输出质量不稳定。

**有了 Skill 后**：你说一句"帮我做调研"，Claude 自动加载调研流程，按标准步骤执行，输出格式统一。

```
你说："帮我调研 RAG 技术"
    │
    ▼
Claude 读取所有 Skill 的 description（每个只有几行）
    │
    ▼
匹配到 ai-research Skill 的描述：
  "Conduct structured AI technology research..."
    │
    ▼
加载完整的 SKILL.md 内容（几十行详细指令）
    │
    ▼
按 Skill 中定义的流程执行：
  Phase 1: 明确范围 → Phase 2: 搜索信息 → Phase 3: 分析 → Phase 4: 输出
```

**关键理解**：Skill 是"导演"，Agent 和 MCP 是"演员"和"道具"。Skill 告诉 Claude 怎么组织整个流程，Agent 和 MCP 负责执行具体动作。

---

## 二、三者如何协作 — 完整流程拆解

以"帮我做一份 AI 技术栈全景调研报告"为例：

```
阶段 0：触发
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
你输入：帮我做一份 AI 技术栈全景调研报告...

Claude 内部判断：
  → 这个任务匹配 ai-research Skill 的 description ✓
  → 加载 Skill 的完整指令
  → 按 Skill 定义的流程开始执行

阶段 1：Skill 指挥 — 明确范围
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claude（主代理）分析你的需求，拆分成子任务：
  1. AI 领域的主要分支
  2. 每个分支的主流框架
  3. 关键术语表
  4. 2025-2026 趋势
  5. 学习路线图

阶段 2：Agent + MCP 执行 — 搜索信息
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

                    Claude 主代理
                   ┌─────┴─────┐
                   │ 拆分任务   │
                   └─────┬─────┘
            ┌────────────┼────────────┐
            ▼            ▼            ▼
      ┌──────────┐ ┌──────────┐ ┌──────────┐
      │ Agent 1  │ │ Agent 2  │ │ Agent 3  │
      │          │ │          │ │          │
      │ 调用 MCP │ │ 调用 MCP │ │ 调用 MCP │
      │ 搜索 NLP │ │ 搜索 CV  │ │ 搜索趋势 │
      │ 框架信息 │ │ 框架信息 │ │ 和动态   │
      └────┬─────┘ └────┬─────┘ └────┬─────┘
           │            │            │
           ▼            ▼            ▼
      返回 NLP 总结  返回 CV 总结  返回趋势总结

每个 Agent 内部可能这样工作：

  Agent 1 的执行过程：
  ├── 调用 Exa MCP：搜索 "NLP framework 2026 comparison"
  ├── 调用 WebSearch MCP：搜索 "自然语言处理 最新框架 2026"
  ├── 调用 DeepWiki MCP：查看 huggingface/transformers 文档
  ├── 调用 Context7 MCP：查 LangChain 最新 API
  └── 综合所有搜索结果 → 输出结构化总结

阶段 3：Claude 主代理 — 分析和综合
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
收到所有 Agent 的结果后：
  → 交叉验证信息
  → 补充自身知识
  → 构建完整的报告框架
  → 填充各章节内容

阶段 4：输出
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  → 按 Skill 定义的格式标准写入 markdown 文件
  → 包含：摘要、术语表、对比表、关系图、来源链接
  → 标注信息时效性（哪些是搜索到的最新信息，哪些是已有知识）
```

---

## 三、你可能的疑问

### Q1：我需要手动创建 Agent 吗？

**不需要。** Claude 会自动判断是否需要派 Agent。当任务足够复杂时，Claude 自己会决定"这个子任务我派个 Agent 去做"。

你能做的是：
- 在 Skill 的 frontmatter 里指定 `agent: Explore`，强制某个 Skill 在子代理中执行
- 但大多数时候，让 Claude 自己决定就好

### Q2：我需要手动调用 MCP 吗？

**不需要。** Claude 会根据需要自动调用。但你可以通过提示影响它的行为：

```
# Claude 可能不搜索（它觉得自己知道答案）
你：什么是 Transformer？

# Claude 一定会搜索（你明确要求了）
你：搜索一下 2026 年最新的 Transformer 变体有哪些

# Claude 会搜索特定来源
你：用 DeepWiki 帮我查一下 vllm 项目的架构文档
```

### Q3：Skill 是必须的吗？

**不是必须的，但强烈推荐。** 区别：

| | 没有 Skill | 有 Skill |
|--|-----------|----------|
| 调研流程 | 每次靠你口头描述 | 预定义的标准流程 |
| 输出格式 | 每次可能不一样 | 统一规范 |
| 可复用性 | 不可复用 | 同事也能用 |
| 质量稳定性 | 取决于你的描述质量 | 稳定一致 |

### Q4：这三者的依赖关系是什么？

```
可以单独用：
  MCP ← 直接让 Claude 搜索信息就好
  Agent ← Claude 处理复杂任务时自动使用
  Skill ← 定义流程但不一定需要搜索或子代理

组合更强：
  Skill + MCP ← 按流程搜索信息
  Skill + Agent ← 按流程拆分子任务
  Skill + Agent + MCP ← 最完整：按流程拆分子任务，每个子任务独立搜索
```

### Q5：实际使用中，我该怎么开始？

**渐进式上手，不要一步到位**：

```
第 1 周：只用直接对话 + MCP（让 Claude 帮你搜索）
         ↓
第 2 周：尝试让 Claude 处理更复杂的任务（它会自动用 Agent）
         ↓
第 3 周：创建你的第一个 Skill（把常用调研流程固化下来）
         ↓
之后：  根据需要安装社区 Plugin，持续优化你的工作流
```

---

## 四、实操：一个完整的调研任务演示

### 你要做的事

调研"RAG（检索增强生成）"技术。

### 第一种方式：纯对话（入门级）

直接输入：
```
帮我全面调研 RAG（Retrieval-Augmented Generation）技术：
1. RAG 是什么，解决什么问题
2. 核心架构和工作流程
3. 主流实现框架（LangChain、LlamaIndex 等）的对比
4. 2025-2026 年的最新进展（搜索最新信息）
5. 给初学者的入门建议

搜索最新资料，写入 rag-research.md
```

**Claude 的行为**：
- 用自身知识写基础内容
- 调用 MCP 搜索最新信息
- 可能自动派 Agent 处理子任务
- 全部写入文件

这已经够用了。不需要任何额外配置。

### 第二种方式：用 Skill 固化流程（进阶级）

当你发现自己经常做类似的调研时，创建一个 Skill：

**文件**: `.claude/skills/tech-research/SKILL.md`

```markdown
---
name: tech-research
description: |
  Deep research on a specific technology topic.
  Trigger when user says "调研", "research", "深入了解" a technology.
  Outputs structured report with latest information.
allowed-tools:
  - Bash
  - Read
  - Write
---

# Technology Deep Research

## Input
The user specifies a technology topic via {{arguments}} or conversation.

## Research Process

### Step 1: Define scope
- What is this technology?
- What problem does it solve?
- Who are the key players?

### Step 2: Gather information
Search using multiple sources:
- Web search for latest articles and news (2025-2026)
- DeepWiki for key GitHub projects
- Context7 for framework documentation
- Cross-reference at least 3 sources for each major claim

### Step 3: Analyze
- Core architecture and how it works
- Compare top 3-5 implementations/frameworks
- Identify trends and future direction
- Note limitations and criticisms

### Step 4: Output format
Write a markdown file with these sections:

```
# {技术名称} 深度调研报告

> 调研日期：{{date}}

## 一句话总结
## 核心概念
## 工作原理（图解）
## 主流框架对比（表格）
## 最新进展（{当前年份}）
## 适用场景 & 不适用场景
## 入门路线
## 参考来源
```

### Step 5: Quality check
- All framework comparisons include at least: 定位、优缺点、适用场景、社区活跃度
- Latest trends section must include search results (not just training knowledge)
- Source URLs included for verifiable claims
- Mark outdated information explicitly
```

之后你每次只需要说：
```
/tech-research RAG
/tech-research AI Agent
/tech-research 向量数据库
```

Claude 就会按照这个标准流程执行，输出格式统一、质量稳定。

### 两种方式的对比

| 维度 | 纯对话 | 用 Skill |
|------|--------|---------|
| 上手难度 | 零配置，直接用 | 需要先写 SKILL.md |
| 输出质量 | 取决于你的提示词质量 | 稳定一致 |
| 可复用 | 每次重新描述 | `/tech-research 主题` 一行搞定 |
| 团队协作 | 不可共享 | Git 提交后团队都能用 |
| 适合场景 | 偶尔做一次 | 经常做同类调研 |

---

## 五、常见误区

| 误区 | 事实 |
|------|------|
| "必须先学会 Skill 才能用 Claude" | 错。直接对话就能完成 90% 的任务 |
| "Agent 需要我手动创建和管理" | 错。Claude 自动决定是否使用 Agent |
| "MCP 需要我配置" | 部分正确。你的环境已经预装了常用 MCP，直接用 |
| "三者必须一起用才有效" | 错。单独用任何一个都有价值，组合只是更强 |
| "Skill 写起来很复杂" | 错。最简单的 Skill 只需要 name + description + 几行指令 |
| "我要一步到位搭建完整工作流" | 错。渐进式上手，先用简单方式，遇到痛点再升级 |

---

## 六、总结

```
三层能力的本质：

  Skill  = 流程（告诉 Claude "按什么步骤做"）
  Agent  = 并行（让 Claude "分身同时做多件事"）
  MCP    = 感知（让 Claude "看到外部世界的信息"）

它们的关系：
  Skill 是导演 → 规划整个流程
  Agent 是演员 → 执行具体子任务
  MCP 是道具   → 提供外部信息和工具

你该怎么做：
  入门 → 直接对话 + 让 Claude 搜索（MCP 自动工作）
  进阶 → 创建 Skill 固化常用流程
  高级 → Skill 编排 Agent 和 MCP 的完整工作流
```
