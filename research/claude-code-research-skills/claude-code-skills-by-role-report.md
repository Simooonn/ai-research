# Claude Code Skills 角色实用性调研报告
> Date: 2026-02-02

## TL;DR

调研覆盖 5 个主要仓库、120+ 个 Skills，按产品经理、前端开发、后端开发、App 端开发、项目经理五个角色分类梳理了对外有用的 Skills。**核心发现**：obra/superpowers 主导开发工作流，daymade 填补非开发场景 gap，anthropics 官方重文档处理，移动端 Skill 严重不足（仅 iOS 有专用 Skill）。

---

## 数据来源

| 仓库 | Stars | 定位 |
|------|-------|------|
| [obra/superpowers](https://github.com/obra/superpowers) | 40.7K | 开发工作流框架，20+ 个 battle-tested skills |
| [daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) | 524 | 35 个生产级 Skill，含中文文档 |
| [anthropics/skills](https://github.com/anthropics/skills) | ~2K+ | 官方参考实现，生产级文档处理 |
| [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | 28.3K | 最大的 Claude Skills 聚合列表 |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 6.3K | 最早的 Claude Skills 聚合列表 |

> Stars 数据来自 2026-02-02 多搜索源交叉验证。

---

## 一、产品经理 (Product Manager)

| Skill | 来源 | 核心价值 | 维护状态 |
|-------|------|----------|---------|
| **brainstorming** | obra/superpowers | 苏格拉底式提问澄清需求，探索替代方案 | ✅ 活跃 |
| **deep-research** | daymade | 市场/竞品调研报告，证据表+引用追踪 | ✅ 活跃 |
| **competitors-analysis** | daymade | 基于源码级分析的竞品对比 | ✅ 活跃 |
| **ppt-creator** | daymade | 金字塔原理演示文稿+数据图表 | ✅ 活跃 |
| **meeting-minutes-taker** | daymade | 转录 → 结构化会议纪要 | ✅ 活跃 |
| **writing-plans** | obra/superpowers | 将 PRD 拆解为工程任务计划 | ✅ 活跃 |
| **internal-comms** | anthropics (官方) | 内部沟通文档模板 | ✅ 官方 |

**推荐工作流**：brainstorming(需求) → deep-research(调研) → competitors-analysis(竞品) → ppt-creator(提案) → meeting-minutes-taker(会议)

---

## 二、前端开发 (Frontend Developer)

| Skill | 来源 | 核心价值 | 维护状态 |
|-------|------|----------|---------|
| **artifacts-builder** | anthropics (官方) | React + Tailwind + shadcn/ui 多组件构建 | ✅ 官方 |
| **frontend-design** | anthropics (官方) | 避免"AI 生成感"美学，大胆设计决策 | ✅ 官方 |
| **ui-designer** | daymade | 从参考截图提取设计系统 → 实现 prompt | ✅ 活跃 |
| **test-driven-development** | obra/superpowers | RED-GREEN-REFACTOR TDD 循环 | ✅ 活跃 |
| **webapp-testing** | anthropics (官方) | Playwright 端到端测试 | ✅ 官方 |
| **i18n-expert** | daymade | i18n 配置 + locale key 一致性审计 | ✅ 活跃 |
| **github-ops** | daymade | PR/Issue/Code Review 自动化 | ✅ 活跃 |
| **mermaid-tools** | daymade | 组件架构图 → PNG | ✅ 活跃 |

**推荐工作流**：ui-designer(设计提取) → artifacts-builder(组件开发) → TDD(测试) → i18n-expert(国际化) → webapp-testing(E2E)

---

## 三、后端开发 (Backend Developer)

| Skill | 来源 | 核心价值 | 维护状态 |
|-------|------|----------|---------|
| **test-driven-development** | obra/superpowers | TDD 严格执行，删除无测试代码 | ✅ 活跃 |
| **systematic-debugging** | obra/superpowers | 4 阶段根因分析 + Defense-in-Depth | ✅ 活跃 |
| **mcp-builder** | anthropics (官方) | 构建 MCP server 集成外部 API | ✅ 官方 |
| **qa-expert** | daymade | Google Testing Standards + OWASP Top 10 | ✅ 活跃 |
| **github-ops** | daymade | PR/Issue/API 自动化 + JIRA 集成 | ✅ 活跃 |
| **promptfoo-evaluation** | daymade | LLM prompt 测试 + 模型对比 | ✅ 活跃 |
| **aws-skills** | 社区 (ComposioHQ) | AWS CDK + serverless 最佳实践 | ✅ 社区 |
| **cloudflare-troubleshooting** | daymade | Cloudflare 诊断排障 | ✅ 活跃 |

**推荐工作流**：TDD(测试先行) → mcp-builder(API 集成) → systematic-debugging(调试) → qa-expert(质量+安全)

---

## 四、App 端开发 (Mobile Developer)

| Skill | 来源 | 核心价值 | 维护状态 |
|-------|------|----------|---------|
| **iOS-APP-developer** | daymade | XcodeGen + SwiftUI + SPM + 签名配置 | ✅ 活跃 |
| **ios-simulator-skill** | 社区 (ComposioHQ) | iOS Simulator 交互测试 | ✅ 社区 |
| **ui-designer** | daymade | 移动端 UI 设计系统提取 | ✅ 活跃 |
| **test-driven-development** | obra/superpowers | 移动端 TDD | ✅ 活跃 |
| **i18n-expert** | daymade | App 国际化配置 | ✅ 活跃 |
| **systematic-debugging** | obra/superpowers | 移动端 bug 根因追踪 | ✅ 活跃 |
| **qa-expert** | daymade | 移动 App QA 测试计划 | ✅ 活跃 |

> ⚠️ **Android 专用 Skill 缺失**：目前仅 iOS 有专用 Skill（daymade 的 iOS-APP-developer），Android/Flutter/React Native 均无对标工具。

**推荐工作流**：iOS-APP-developer(项目配置) → ui-designer(设计) → TDD(测试) → i18n-expert(国际化) → ios-simulator-skill(调试)

---

## 五、项目经理 (Project Manager)

| Skill | 来源 | 核心价值 | 维护状态 |
|-------|------|----------|---------|
| **writing-plans** | obra/superpowers | 详细实现计划生成（2-5 分钟粒度任务） | ✅ 活跃 |
| **executing-plans** | obra/superpowers | 批量执行 + checkpoint 追踪 | ✅ 活跃 |
| **subagent-driven-development** | obra/superpowers | 并行任务分配 + 两阶段 review | ✅ 活跃 |
| **meeting-minutes-taker** | daymade | 会议纪要自动生成 | ✅ 活跃 |
| **github-ops** | daymade | Issue tracking + PR management | ✅ 活跃 |
| **qa-expert** | daymade | 质量门禁监控 | ✅ 活跃 |
| **statusline-generator** | daymade | Claude 使用成本追踪+进度可视化 | ✅ 活跃 |
| **teams-channel-post-writer** | daymade | 团队知识分享公告 | ✅ 活跃 |

**推荐工作流**：writing-plans(计划) → subagent-driven-development(并行执行) → meeting-minutes-taker(会议) → qa-expert(质量)

---

## 六、跨角色通用 Skills

这 5 个 Skill 对所有角色都有价值：

| Skill | 来源 | 说明 |
|-------|------|------|
| **github-ops** | daymade | PR/Issue 自动化，每个角色日常都用 |
| **mermaid-tools** | daymade | 架构图/流程图文档化 |
| **pdf-creator** | daymade | Markdown → PDF（支持中文字体） |
| **docx/xlsx/pptx** | anthropics (官方) | Office 文档处理三件套 |
| **skill-creator** | daymade / anthropics | 创建团队定制 Skill 的元工具 |

---

## 七、关键发现

### 生态格局

1. **obra/superpowers 主导开发工作流**：TDD、调试、计划执行等核心开发 Skill 最成熟（40.7K ⭐）
2. **daymade 填补非开发 gap**：QA、i18n、媒体处理、会议纪要等官方和 obra 未覆盖的领域（35 个 Skill）
3. **anthropics 官方重文档处理**：docx/pdf/pptx/xlsx 四件套是生产级参考实现
4. **移动端 Skill 严重不足**：仅 iOS 有专用 Skill，Android/Flutter/RN 均缺失

### 各角色受益程度

| 角色 | 可用 Skill 数 | 成熟度 | 说明 |
|------|--------------|--------|------|
| 前端开发 | 8 | ⭐⭐⭐⭐⭐ | 官方 3 个 + 社区覆盖完整，工具链最成熟 |
| 后端开发 | 8 | ⭐⭐⭐⭐ | TDD + 调试 + MCP 核心链路完整，云服务 Skill 有待增强 |
| 项目经理 | 8 | ⭐⭐⭐⭐ | 计划→执行→会议→质量链路完整，obra 的工作流框架加分 |
| 产品经理 | 7 | ⭐⭐⭐ | 调研+提案链路可用，但调研类 Skill 仍处早期 |
| App 端开发 | 7 | ⭐⭐ | 仅 iOS 有专用 Skill，Android 完全空白，生态最薄弱 |

### 机会点

- **Android / 跨平台移动开发 Skill**：当前完全空白，是明确的生态缺口
- **产品经理专用 Skill**：用户故事生成、需求优先级排序、A/B 测试设计等场景无专用 Skill
- **项目管理集成**：与 JIRA/Linear/Notion 等项目管理工具的深度集成 Skill 稀缺

---

## Sources

### 核心仓库
- [obra/superpowers](https://github.com/obra/superpowers) — 40.7K ⭐，开发工作流框架
- [daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) — 524 ⭐，35 个生产级 Skill
- [anthropics/skills](https://github.com/anthropics/skills) — ~2K+ ⭐，官方参考实现

### Awesome 列表
- [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) — 28.3K ⭐
- [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) — 6.3K ⭐

### 官方资源
- [Anthropic Skills 公告](https://www.anthropic.com/news/skills)
- [Skills Explained 详解](https://claude.com/blog/skills-explained)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
