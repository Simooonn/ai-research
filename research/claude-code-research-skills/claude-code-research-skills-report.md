# Claude Code 调研类 Skill 深度调研报告
> Date: 2026-02-01

## TL;DR

Claude Code 的 Skill 生态在 2025 年 10 月正式发布后爆发式增长。**调研类 Skill 目前仍属早期赛道**，能够通过社区验证、持续维护、且有实际技术深度的选手非常少。经过严格筛选（排除低 stars、无维护、一次性项目、自荐无验证、非调研定位的项目），真正值得核心对比的只有 **1 个**：

1. **daymade/deep-research**（所属仓库 524 ⭐，2026-01-25 新发布）— 多 pass UNION 合并 + 证据表 + 格式契约，方法论最完整

199-biotechnologies/claude-deep-research-skill 技术设计上有亮点（8.5 阶段管线、CiteGuard 引用验证等），但**全部 19 个 commit 集中在 2 天内（2025-11-04~05），之后零更新**，属于一次性 AI 辅助生成项目，已降级为参考项目。

建议我们的 tech-research skill **取长补短**：保持现有联网搜索 + 中文输出 + 双文档策略的优势，借鉴社区在子问题拆解、来源质量评级、格式契约方面的最佳实践。

## 关键问题回答

### Q: 有没有比较好的调研功能 Skill？
**有，但选择非常有限。** 经严格筛选，只有 1 个值得核心参考（详见对比表）。调研类 Skill 整体成熟度远不如开发工具类 Skill。

### Q: 评判标准是什么？
综合评估了以下维度，**不达标的直接排除**：
- ⭐ **GitHub Stars + 社区活跃度**：排除 <10 stars 且无持续维护的项目
- 🔄 **更新频率**：排除长期无更新、停止维护的项目
- 🏗️ **技术实现深度**：排除"包了一句 prompt"等无实际价值的封装
- 📋 **社区验证**：排除仅作者自荐、无第三方引用的项目
- 🎯 **调研定位**：排除本质不是调研工具但被错误归类的项目

### 筛选过程中被排除的项目

| 项目 | Stars | 排除理由 |
|------|-------|----------|
| 199-biotechnologies/claude-deep-research-skill | 23 ⭐ | **一次性项目**：全部 19 commits 集中在 2 天内（2025-11-04~05），之后零更新。疑似 AI 辅助一次性生成。技术设计有参考价值（CiteGuard、动态大纲等），但无持续维护 → 降为参考项目 |
| hikarubw/claude-commands `/research` | 8 ⭐ | 9 commits，长期无更新，本质只是一句"用 ultrathink 模式"的 prompt 封装，直接让 Claude 深度思考即可达到同样效果 |
| sanjay3290/ai-skills `deep-research` | 极低 | 作者在 awesome-claude-code 自荐（[Issue #466](https://github.com/hesreallyhim/awesome-claude-code/issues/466)），社区无独立验证，依赖 Gemini API 增加外部成本 |
| willccbb/claude-deep-research | 225 ⭐ | 仅 9 commits，本质是一个 shell 脚本 + MCP 配置文件，不是真正的 Skill 实现 |
| glebis/claude-skills `deep-research` | 低 | 依赖 OpenAI Deep Research API，属于 API 封装，非独立调研能力 |
| obra/superpowers `brainstorming` | (母仓库 40.7K ⭐) | **不是调研 Skill**，是开发前的需求探索/设计工具，放在调研对比中属于错误归类。其苏格拉底式提问方法值得借鉴，但定位完全不同 |

---

## 一、Claude Code Skill 生态全景

### 1.1 Skill 是什么

Claude Code Skills 是 Anthropic 于 **2025 年 10 月 16 日**正式发布的功能，它是包含指令、脚本和资源的文件夹，教会 Claude 以可重复的方式执行特定任务。

**核心架构——渐进式披露（Progressive Disclosure）：**
1. **元数据扫描**（~100 tokens）：Claude 扫描所有可用 Skill 识别相关匹配
2. **完整指令加载**（<5K tokens）：Claude 确定 Skill 适用时加载
3. **捆绑资源**：文件和可执行代码按需加载

> 来源：[Anthropic Skills 公告](https://www.anthropic.com/news/skills)、[Skills 详解](https://claude.com/blog/skills-explained)

### 1.2 生态主要参与者（已验证数据）

| 仓库 | Stars | Forks | 类型 | 更新频率 | 说明 |
|------|-------|-------|------|----------|------|
| [**obra/superpowers**](https://github.com/obra/superpowers) | **40.7K** | 3.1K | 开发工作流框架 | 非常活跃 | 🔥 最火的 Skill 框架，完整自主开发工作流 |
| [**ComposioHQ/awesome-claude-skills**](https://github.com/ComposioHQ/awesome-claude-skills) | **28.3K** | 2.7K | Awesome 聚合列表 | 活跃 | 最大的 Claude Skills 聚合列表 |
| [**K-Dense-AI/claude-scientific-skills**](https://github.com/K-Dense-AI/claude-scientific-skills) | **7.2K** | 865 | 科学研究 Skills | 活跃 | 科学领域专用 Skills 集 |
| [**travisvn/awesome-claude-skills**](https://github.com/travisvn/awesome-claude-skills) | **6.3K** | 409 | Awesome 聚合列表 | 活跃 | 最早的 Claude Skills 聚合列表，含官方 Skill 索引 |
| [**daymade/claude-code-skills**](https://github.com/daymade/claude-code-skills) | **524** | 57 | Skills 市场 | 非常活跃 | 35 个生产级 Skill，含中文文档，含 `deep-research` |
| [**anthropics/skills**](https://github.com/anthropics/skills) | ~2K+ | ~500+ | 官方 Skills | 活跃 | Anthropic 官方（docx, pdf, pptx, xlsx 等） |
| [**VoltAgent/awesome-agent-skills**](https://github.com/VoltAgent/awesome-agent-skills) | 172+ Skills | — | 跨平台 Awesome 列表 | 活跃 | 跨 Claude Code/Codex/Gemini CLI/Cursor 等 |

> ⚠️ Stars 数据来自 2026-02-01 多搜索源交叉验证，obra/superpowers 在 2026 年 1 月经历爆发式增长（3 天 +5167 stars）。

### 1.3 Skills 市场 / 目录平台

| 平台 | 说明 | 质量控制 |
|------|------|----------|
| [**SkillsMP.com**](https://skillsmp.com/) | 96,751+ Skills 目录，从公开 GitHub 仓库抓取 | 最低 2 stars 门槛 + 基础质量扫描 |
| [**skills.rest**](https://skills.rest/) | 社区贡献的可执行 Skill | 人工审核 |
| [**claude-plugins.dev**](https://claude-plugins.dev/) | 社区维护的 Skill 市场 | 社区审核 |

---

## 二、调研类 Skill 核心对比

### 2.1 唯一通过筛选的调研 Skill

经过严格的活跃度筛选，**只有 1 个调研类 Skill 通过了全部筛选标准**。这反映了调研类 Skill 赛道的早期状态——大量项目要么是一次性生成、要么是 prompt 封装、要么已停止维护。

| 维度 | **daymade/deep-research** |
|------|--------------------------|
| **类型** | Skill（SKILL.md，Plugin 市场分发） |
| **所属仓库 Stars** | 524 ⭐（母仓库 35 个 Skill） |
| **发布时间** | 2026-01-25（v1.26.0 新增） |
| **Commits** | 活跃（母仓库 70+） |
| **定位** | 格式控制的调研报告生成 |
| **核心管线** | 9 步工作流 |
| **核心特色** | 多 pass UNION 合并 + 证据表 + 格式契约 |
| **调研模式** | 单一模式 |
| **输出格式** | Markdown/DOCX/Slides（可模板化） |
| **引用管理** | ✅ 证据表 + 引用验证 |
| **来源评级** | ✅ 来源质量评估标准（rubric） |
| **多 pass 合并** | ✅ 并行子 agent UNION 合并 |
| **联网搜索** | 依赖 deepresearch 工具 |
| **中文支持** | ✅ 仓库有中文文档 |
| **配套脚本** | ❌ 纯指令型 |
| **安装方式** | Plugin Marketplace 一键安装 |

### 2.2 详细分析

#### 🥇 daymade/deep-research — 唯一通过筛选，方法论最完整

**仓库**：[daymade/claude-code-skills](https://github.com/daymade/claude-code-skills)（524 ⭐, 57 Forks）
**发布**：2026-01-25（v1.26.0 新增）

**核心工作流（9 步）：**
1. **Intake & Format Contract** — 确认受众、目的、范围、时间、地域、引用格式、长度目标
2. **Research Plan & Query Set** — 拆分 3-7 个子问题，定义关键词和消歧器
3. **Evidence Collection** — 使用 deepresearch 工具多 pass 收集
4. **Source Triage & Evidence Table** — 来源质量评估和证据表构建
5. **Outline & Section Map** — 大纲和章节映射
6. **Multi-pass Full Drafting** — 并行子 agent 多次完整起草
7. **UNION Merge & Format Compliance** — 合并所有 pass 的内容，强制格式合规
8. **Evidence & Citation Verification** — 引用和证据验证
9. **Present for Human Review** — 提交人工审查并迭代

**值得借鉴的设计：**
- 📊 **证据表机制**：每个事实声明都有来源追踪，可审计
- 🔄 **多 pass UNION 合并**：并行多个子 agent 各写一遍完整报告，然后合并，避免遗漏
- 📋 **格式契约（Format Contract）**：调研前锁定输出格式、引用风格、每节长度目标；用户提供模板则作为硬约束
- 🎯 **子问题拆解**：将调研主题拆为 3-7 个子问题，每个子问题有独立的查询集
- 📁 配套参考文档：含 `research_report_template.md`、`source_quality_rubric.md`

**局限：**
- 依赖 `deepresearch` 工具（需要 Claude Code 内置或 MCP 配置），不能直接联网搜索
- 刚发布（2026-01-25），社区实际使用反馈尚不充分
- 纯指令型 Skill，无配套脚本

> 来源：[deep-research/SKILL.md](https://github.com/daymade/claude-code-skills/blob/main/deep-research/SKILL.md)

---

### 2.3 参考项目（未通过核心筛选，但有借鉴价值）

#### 199-biotechnologies/claude-deep-research-skill — 技术设计有亮点，但属一次性项目

**仓库**：[199-biotechnologies/claude-deep-research-skill](https://github.com/199-biotechnologies/claude-deep-research-skill)（23 ⭐, 4 Forks）
**版本**：v2.2 | **活跃度**：⚠️ 全部 19 commits 集中在 2025-11-04~05（2 天），此后零更新

> ⚠️ **未通过活跃度筛选**：虽然技术设计有参考价值，但该项目属于典型的"一次性 AI 辅助生成"模式——密集提交 2 天后完全停止维护，无 issue 讨论、无社区互动、无后续迭代。不建议直接采用，仅提取设计理念供参考。

**可借鉴的设计理念：**
- 🔬 **多调研模式**：Quick/Standard/Deep/UltraDeep 按需选择
- 🔍 **CiteGuard 引用验证**：幻觉检测 + URL 验证 + 多源交叉校验
- 📋 **动态大纲演化（WebWeaver）**：根据实际证据调整报告结构
- 📄 **自动续写系统**：递归 agent 生成超长报告
- 📚 **学术论文引用**：GAP, CiteGuard, WebWeaver 等 2025 论文

### 2.4 其他值得了解的相关工具（仅作参考，非核心对比）

| 项目 | Stars | 说明 | 参考价值 |
|------|-------|------|----------|
| [**obra/superpowers**](https://github.com/obra/superpowers) `brainstorming` | 40.7K ⭐ | 不是调研工具，但其苏格拉底式提问方法值得借鉴 | 调研前需求澄清 |
| [**K-Dense-AI/claude-scientific-skills**](https://github.com/K-Dense-AI/claude-scientific-skills) | 7.2K ⭐ | 科学研究领域专用 Skills 集，非通用调研 | 科学调研场景 |
| [**daymade/competitors-analysis**](https://github.com/daymade/claude-code-skills) | (同仓库) | 基于证据的竞品分析，必须克隆源码分析 | 竞品调研方法论 |
| [**daymade/fact-checker**](https://github.com/daymade/claude-code-skills) | (同仓库) | 文档事实核查，可作为调研后验证环节 | 报告质量保证 |

---

## 三、与我们 tech-research Skill 的对比分析

### 3.1 我们的 tech-research 做得好的

| 特点 | 说明 |
|------|------|
| ✅ **联网搜索** | 使用 Web Search、DeepWiki、Context7、Exa 多源搜索——这是两个竞品都不具备的 |
| ✅ **中文输出** | 默认中文简体输出，代码/术语保留英文 |
| ✅ **文件组织规范** | 有明确的 `research/{topic}/` 目录规范 |
| ✅ **双文档策略** | 可部署技术产出调研报告 + 部署指南，竞品均无此设计 |
| ✅ **自包含管线** | 部署指南要求每种方案完整自包含 |
| ✅ **交叉验证** | 要求至少 3 个来源交叉验证重要声明 |

### 3.2 可以从社区 Skill 借鉴的

| 借鉴点 | 来源 | 说明 |
|--------|------|------|
| 📊 **证据表机制** | daymade/deep-research | 为每个事实声明建立来源追踪表，提高可审计性 |
| 📋 **格式契约（Format Contract）** | daymade/deep-research | 调研前锁定输出格式、引用风格、每节长度目标 |
| 🎯 **子问题拆解** | daymade/deep-research | 将调研主题拆为 3-7 个子问题，提高覆盖面 |
| 📁 **来源质量评级** | daymade + 199-biotech | 对来源进行权威性、时效性、相关性评级 |
| 🔬 **多调研模式** | 199-biotech（参考） | Quick/Standard/Deep 不同深度，按需选择（理念可借鉴，但原项目已停止维护） |
| 🔍 **引用验证** | 199-biotech CiteGuard（参考） | 防止幻觉引用，URL 可达性检查（设计理念好，需自行实现） |
| 📋 **动态大纲调整** | 199-biotech WebWeaver（参考） | 调研中途根据实际证据调整报告结构（理念可借鉴） |

### 3.3 优化建议优先级

| 优先级 | 建议 | 预期收益 |
|--------|------|----------|
| 🔴 高 | 添加**子问题拆解**步骤：调研前先将主题分解为 3-7 个子问题 | 提高调研覆盖面和结构性 |
| 🔴 高 | 添加**来源质量标注**：标记每个来源的类型（官方/社区/个人博客）和可信度 | 提高报告可信度 |
| 🔴 高 | 添加**活跃度/维护状态筛选标准**：对比项目时必须检查 stars、更新频率、commit 数量，排除无维护项目 | 避免本次调研中出现的"把无价值项目拉入核心对比"问题 |
| 🟡 中 | 添加**格式契约**步骤：调研前确认输出格式、长度、引用格式 | 减少返工 |
| 🟢 低 | 考虑**多调研模式**：Quick（快速扫描）vs Deep（深度调研） | 灵活适配不同需求 |
| 🟢 低 | 支持**证据表**输出：可选的来源追踪附件 | 提高学术性和可追溯性 |

---

## 四、Skill 生态最新动态（2025-2026）

### 4.1 关键时间线

| 时间 | 事件 |
|------|------|
| 2025-10-16 | 🎉 Anthropic 正式发布 Claude Skills |
| 2025-10-18 | obra/superpowers 社区仓库出现 |
| 2025-11-04~05 | 199-biotechnologies 发布 claude-deep-research-skill v1.0~v2.2（2 天内完成全部开发，此后停止更新） |
| 2025-11-13 | Anthropic 发布 [Skills Explained](https://claude.com/blog/skills-explained) 详解 |
| 2025-12 | Agent Skills 规范成为开放标准，OpenAI Codex CLI 采用相同格式 |
| 2026-01-13~16 | obra/superpowers 爆发增长（3 天 +5167 stars），登顶 GitHub Trending |
| 2026-01-25 | daymade/claude-code-skills 发布 v1.26.0（新增 deep-research skill）|
| 2026-01-28 | 36 Claude Skills Examples 汇总文章获广泛传播 |

### 4.2 趋势观察

1. **跨平台兼容**：Agent Skills 格式正在成为事实标准，Claude Code、Codex、Gemini CLI、Cursor、Windsurf、GitHub Copilot 等均支持
2. **从 Skills 到 Plugins**：Claude Code 2.0.13+ 引入 Plugin Marketplace 机制，支持一键安装
3. **官方团队入场**：Vercel、Cloudflare、Stripe、Supabase、HuggingFace、Expo、Sentry 等均发布官方 Skills
4. **调研类 Skill 仍处早期**：相比开发工具类 Skill 的成熟度，调研类 Skill 选择有限且质量参差不齐
5. **质量 > 数量**：SkillsMP 有 96K+ 条目，但大量是低质量/无维护的，需严格筛选

---

## 五、总结

### 核心结论

1. **调研类 Skill 赛道早期**，通过活跃度筛选的只有 **1 个**（daymade/deep-research），199-biotech 因一次性项目特征已降为参考
2. **我们的 tech-research 已有独有优势**（联网搜索、中文、双文档策略），不需要替换
3. **取长补短是最优策略**：重点借鉴子问题拆解、来源质量评级、格式契约
4. **质量筛选标准很重要**：本次调研初版犯了"把无维护/低价值项目拉入核心对比"的错误，未来调研应将活跃度筛选前置

### 对 SKILL.md 的建议更新

在调研过程（Step 3: Analyze and synthesize）中增加：
- **活跃度筛选前置**：对比项目前必须验证 stars、更新频率、commit 数量
- **排除标准明确化**：<10 stars + 无持续维护 + 纯 prompt 封装 → 直接排除或仅作脚注
- **子问题拆解**：Step 2 增加"将调研主题拆分为 3-7 个子问题"

---

## Sources

### 官方资源
- [Anthropic Skills 公告](https://www.anthropic.com/news/skills)
- [Skills Explained 详解](https://claude.com/blog/skills-explained)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [anthropics/skills 官方仓库](https://github.com/anthropics/skills)

### 核心对比仓库
- [daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) — 524 ⭐，Skills 市场（含 deep-research，2026-01-25 发布）

### 参考仓库（有设计借鉴价值，但未通过活跃度筛选）
- [199-biotechnologies/claude-deep-research-skill](https://github.com/199-biotechnologies/claude-deep-research-skill) — 23 ⭐，一次性项目（全部 commit 集中在 2 天内），技术设计理念可参考

### Awesome 列表（已验证 Stars）
- [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) — 28.3K ⭐
- [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) — 6.3K ⭐
- [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) — 172+ Skills 跨平台聚合

### 其他参考仓库
- [obra/superpowers](https://github.com/obra/superpowers) — 40.7K ⭐，开发工作流框架（brainstorming 方法可借鉴）
- [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) — 7.2K ⭐，科学研究 Skills

### 社区文章
- [Simon Willison: Claude Skills are awesome](https://simonwillison.net/2025/Oct/16/claude-skills/) — Skills 技术深度分析
- [Superpowers Blog](https://blog.fsck.com/2025/10/09/superpowers/) — Jesse Vincent 的 Superpowers 开发哲学
- [36 Claude Skills Examples](https://aiblewmymind.substack.com/p/claude-skills-36-examples) — 36 个 Skill 示例汇总（2026-01-25）
- [GitHub Trending: superpowers 现象](https://medium.com/@lssmj2014/github-trending-january-16-2026-superpowers-phenomenon-local-first-ai-4dc2b02e173a)
