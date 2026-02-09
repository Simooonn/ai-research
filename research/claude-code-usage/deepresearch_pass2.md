# Claude Code 使用方式调研 - 第二轮证据收集

## 搜索时间
2026-02-10

## 搜索主题
- CLAUDE.md 配置最佳实践
- Hooks 钩子系统应用
- 常见问题与故障排查

## 主要发现来源

### CLAUDE.md 配置专题
- Claude Code 完全指南 (CLAUDE.md详解): https://www.cnblogs.com/knqiufan/p/19449849
- CLAUDE.md 最佳实践 8条黄金法则: https://damodev.csdn.net/696c8c73a16c6648a98333c7.html
- 从0到1搭建项目环境: https://cloud.tencent.com/developer/article/2596824
- Mintlify 文档配置指南: https://www.mintlify.com/docs/zh/guides/claude-code
- 官方内存管理文档: https://code.claude.com/docs/zh-CN/memory
- Claude 中文网内存管理: https://www.claude-cn.org/claude-code-docs-zh/configuration/memory
- Claude Code Hub 内存管理: https://claudecodehub.github.io/memory.html

### 常见问题与故障排查
- Skills Fan FAQ (详细问题分类): https://www.skills.fan/docs/troubleshooting/faq
- Claude Code FAQ (完整故障排查): https://hrefgo.com/zh/blog/claude-code-faq-troubleshooting-support
- 官方故障排除文档 (中文): https://code.claude.com/docs/zh-TW/troubleshooting
- Anthropic 官方故障排除: https://docs.anthropic.com/zh-CN/docs/claude-code/troubleshooting
- 架构层面故障排除指南: https://cloud.tencent.com/developer/article/2555179
- Claude Code Pro FAQ: https://claudecode.pro/zh/docs/faq

### 调试与优化
- Day 12 傻瓜式调试技巧: https://medium.com/@marklaik/day-12-%E5%82%BB%E7%93%9C%E5%BC%8F%E8%AA%BF%E9%81%A9-claude-code%E7%94%A2%E7%94%9F%E7%9A%84bugs-b6f4777c452e

## 核心发现

### CLAUDE.md 配置要点
1. **三层内存架构**:
   - 企业策略 (组织级)
   - 项目内存 (团队共享)
   - 用户内存 (个人偏好)

2. **最佳实践原则**:
   - 保持简短精炼
   - 使用导入语法 (@path/to/import)
   - 分层递归查找
   - 模块化规则 (.claude/rules/)

3. **配置内容建议**:
   - 项目架构说明
   - 编码规范
   - 常用工作流
   - 工具权限管理

### 常见问题分类

#### 安装类
- Node.js 版本不兼容 (需要 18+)
- PATH 环境变量问题
- WSL 路径冲突
- 权限错误

#### 配置类
- API Key 无效
- 登录状态丢失
- 浏览器未自动打开

#### 运行时
- 高 CPU/内存占用
- 命令挂起或冻结
- 搜索结果缓慢

#### IDE 集成
- JetBrains 未检测
- WSL2 网络模式问题
- VS Code 终端问题

### 故障排查流程
1. 检查系统环境 (Node.js, npm)
2. 验证 API 凭证
3. 查看配置文件完整性
4. 重置配置 (必要时)
5. 查看日志输出

## 信息完整性评估
✅ 基础安装配置
✅ 核心功能详解
✅ CLAUDE.md 最佳实践
✅ Hooks 系统应用
✅ 常见问题解决
✅ 故障排查流程
✅ 性能优化建议
✅ 实战案例收集

## 下一步
开始整理报告大纲和证据表
