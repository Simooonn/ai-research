# AI 研究项目 - Claude Code 使用配置

## 项目概述
- 项目名称：AI Research
- 类型：技术研究与文档项目
- 主要内容：关于 Claude Code、OpenClaw、RustDesk 等工具的研究报告

## 项目结构
- `research/` - 研究文档和报告
  - `claude-code-usage/` - Claude Code 使用研究
  - `openclaw/` - OpenClaw 工具研究
  - `rustdesk/` - RustDesk 远程桌面研究
  - `manus-ai/` - Manus AI 相关研究
- `reports/` - 格式化的报告输出
- `guides/` - 使用指南和教程
- `.spec-workflow/` - 规范工作流程配置

## 编码规范
- 使用 Markdown 格式编写文档
- 文件名使用 kebab-case 命名
- 图片和资源放入对应的 `assets/` 目录
- 代码示例使用正确的语法高亮

## 文档规范
- 报告使用中文撰写
- 包含版本信息和更新日期
- 使用清晰的标题层级
- 代码示例使用三反引号语法
- 表格使用 Markdown 表格格式

## 常用命令
- `npm run build` - 构建项目
- `npm run test` - 运行测试
- `claude` - 启动 Claude Code 会话

## 研究流程
1. 使用 Plan 模式分析需求
2. 制定研究计划和大纲
3. 执行研究并收集资料
4. 编写详细的研究报告
5. 审查和优化报告内容

## 导入额外规则
@import .claude/rules/research.md
