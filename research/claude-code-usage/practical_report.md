# Claude Code 实用调研报告：代码示例与问题排查

> **版本**: V1.0 | **日期**: 2026-02-13 | **字数**: 约8,000字
> **适用版本**: Claude Code 最新稳定版 | **语言**: 简体中文

---

## 目录

- [第一部分：快速上手](#第一部分快速上手)
  - [1.1 安装与验证](#11-安装与验证)
  - [1.2 常用启动方式](#12-常用启动方式)
- [第二部分：核心功能代码示例](#第二部分核心功能代码示例)
  - [2.1 文件操作](#21-文件操作)
  - [2.2 代码生成](#22-代码生成)
  - [2.3 代码重构](#23-代码重构)
  - [2.4 测试生成](#24-测试生成)
  - [2.5 Git 操作](#25-git操作)
- [第三部分：问题排查与调试](#第三部分问题排查与调试)
  - [3.1 安装问题](#31-安装问题)
  - [3.2 配置问题](#32-配置问题)
  - [3.3 运行时问题](#33-运行时问题)
  - [3.4 性能问题](#34-性能问题)
  - [3.5 调试技巧](#35-调试技巧)
- [第四部分：实战案例](#第四部分实战案例)
  - [4.1 快速开发 Todo 应用](#41-快速开发-todo-应用)
  - [4.2 代码审查自动化](#42-代码审查自动化)
  - [4.3 多 Claude 协作开发](#43-多-claude-协作开发)
- [第五部分：最佳实践](#第五部分最佳实践)
  - [5.1 CLAUDE.md 编写技巧](#51-claude-md-编写技巧)
  - [5.2 权限管理](#52-权限管理)
  - [5.3 上下文优化](#53-上下文优化)
- [附录：常用命令速查表](#附录常用命令速查表)

---

# 第一部分：快速上手

## 1.1 安装与验证

### 1.1.1 系统要求检查

在安装前先检查系统是否符合要求：

```bash
# 检查 Node.js 版本（要求 >= 18.0.0）
node --version

# 检查 npm 版本
npm --version

# 检查操作系统信息
uname -a  # macOS/Linux
systeminfo  # Windows (PowerShell)
```

### 1.1.2 安装命令

**推荐安装方式（macOS/Linux）：**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**npm 全局安装：**
```bash
npm install -g @anthropic-ai/claude-code
```

**验证安装：**
```bash
# 检查版本
claude --version

# 启动并测试
claude -p "你好，请介绍一下自己"
```

## 1.2 常用启动方式

```bash
# 在当前项目根目录启动（推荐）
claude

# 单次任务模式（执行完即退出）
claude -p "解释这个项目的架构"

# 指定项目目录启动
claude --project /path/to/your/project

# 恢复上一次会话
claude --resume

# 管道输入模式
cat error.log | claude "分析这个错误日志"

# 计划模式（只分析不执行）
claude --permission-mode plan

# 自动模式（无需确认）
claude --dangerously-skip-permissions
```

---

# 第二部分：核心功能代码示例

## 2.1 文件操作

### 2.1.1 读取文件

```bash
# 读取整个文件
> 阅读 src/index.ts 文件并解释其功能

# 读取特定范围
> 读取 src/auth/jwt.ts 的第 20-50 行

# 读取特定函数
> 阅读 src/services/user.service.ts 中的 createUser 函数
```

### 2.1.2 搜索与匹配

```bash
# 搜索文件
> 找到项目中所有以 Auth 开头的 .tsx 文件

# 搜索内容
> 在所有 .ts 文件中查找使用 moment.js 的代码

# 正则搜索
> 查找包含 console.log 的代码行
```

### 2.1.3 编辑文件

```bash
# 替换内容
> 在所有 .ts 文件中，将 console.log 替换为 logger.info

# 插入代码
> 在 src/app.ts 中添加错误处理中间件

# 重构组件
> 将 src/components/OldComponent.tsx 重构为函数式组件
```

## 2.2 代码生成

### 2.2.1 创建新文件

```bash
# 创建工具函数
> 创建一个 src/utils/logger.ts 日志工具模块，包含 info、error、debug 方法

# 创建 API 接口
> 创建一个用户认证的 API 接口，包含登录、注册、刷新 token 功能

# 创建配置文件
> 创建 .eslintrc.json 配置文件，使用 standard 规范
```

### 2.2.2 完整功能生成

```bash
# 生成 Todo 应用
> 创建一个完整的 React + TypeScript + Vite 项目，包含 Todo 列表功能

# 生成后端服务
> 实现一个 Express.js REST API，包含用户 CRUD 操作和 JWT 认证

# 生成数据库模型
> 使用 Prisma 生成用户和文章的数据库模型
```

## 2.3 代码重构

### 2.3.1 架构重构

```bash
# 拆分大文件
> 将 src/app.ts 拆分为独立的模块，遵循 SOLID 原则

# 迁移技术栈
> 将项目从 JavaScript 迁移到 TypeScript

# 库版本升级
> 将项目中所有使用 moment.js 的地方迁移到 dayjs
```

### 2.3.2 代码优化

```bash
# 性能优化
> 优化 src/utils/date.ts 中的日期格式化函数，提升性能

# 类型优化
> 为 src/types/user.ts 添加完整的 TypeScript 类型定义

# 错误处理优化
> 完善 src/middleware/errorHandler.ts 的错误处理逻辑
```

## 2.4 测试生成

### 2.4.1 单元测试

```bash
# 为函数生成测试
> 为 src/utils/calculateDiscount.ts 编写单元测试，覆盖所有边界条件

# 为组件生成测试
> 为 src/components/UserProfile.tsx 编写 React 测试

# 为 API 生成测试
> 为用户认证 API 编写集成测试
```

### 2.4.2 测试驱动开发 (TDD)

```bash
# 1. 编写失败的测试
> 为 calculateDiscount 函数编写测试用例，考虑以下场景：
> - 普通用户 10% 折扣
> - VIP 用户 20% 折扣
> - 折扣金额不超过 100 元
> - 负数金额抛出错误

# 2. 运行测试确认失败
> 运行测试确认这些测试用例目前都会失败

# 3. 编写通过代码
> 现在实现 calculateDiscount 函数，使所有测试通过

# 4. 重构优化
> 测试全部通过了，请重构 calculateDiscount 的实现，提升可读性
```

## 2.5 Git 操作

### 2.5.1 自动 Commit

```bash
# 自动生成 Commit 信息
> /commit

# 自定义 Commit 信息
> /commit -m "feat: 添加用户认证功能" -d "实现了 JWT 认证和用户管理"
```

### 2.5.2 PR 管理

```bash
# 一键创建 PR
> /pr

# 审查 PR
> 审查 GitHub PR #42 的所有变更，重点检查安全问题和性能影响
```

### 2.5.3 分支管理

```bash
# 基于当前分支创建新分支
> 创建一个新分支 feature/user-profile 并切换到该分支

# 合并分支
> 将 feature/user-profile 分支合并到 develop 分支，并处理冲突
```

---

# 第三部分：问题排查与调试

## 3.1 安装问题

### 3.1.1 命令找不到 (command not found)

**问题现象：**
```bash
claude: command not found
```

**解决方案：**

```bash
# 检查安装位置
which claude
ls ~/.local/bin/claude

# 检查 npm 全局安装路径
npm config get prefix

# 将安装目录添加到 PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc  # macOS
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc  # Linux

# 刷新配置
source ~/.zshrc
```

### 3.1.2 Node.js 版本不兼容

**问题现象：**
```bash
Error: Unsupported Node.js version. Please use Node.js >= 18.0.0
```

**解决方案：**

```bash
# 安装 nvm（Node 版本管理器）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# 安装并使用 Node.js 20 LTS
nvm install 20
nvm use 20
nvm alias default 20

# 验证版本
node --version
```

### 3.1.3 权限错误 (EACCES)

**问题现象：**
```bash
Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/@anthropic-ai'
```

**解决方案：**

```bash
# 修复 npm 全局安装权限
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 重新安装
npm install -g @anthropic-ai/claude-code
```

## 3.2 配置问题

### 3.2.1 API Key 无效

**问题现象：**
```bash
Error: Invalid API key. Please check your configuration.
```

**解决方案：**

```bash
# 检查 API Key 是否设置
echo $ANTHROPIC_API_KEY

# 验证 API Key 格式（以 sk-ant- 开头）
if [[ $ANTHROPIC_API_KEY != sk-ant-* ]]; then
  echo "API Key 格式不正确"
fi

# 测试 API 连通性
curl -H "x-api-key: $ANTHROPIC_API_KEY" \
  https://api.anthropic.com/v1/messages \
  -d '{"model":"claude-sonnet-4-20250514","max_tokens":10,"messages":[{"role":"user","content":"test"}]}'
```

### 3.2.2 配置文件权限问题

**问题现象：**
```bash
Error: Failed to read CLAUDE.md: EACCES: permission denied
```

**解决方案：**

```bash
# 检查配置文件权限
ls -la ~/.claude/
ls -la .claude/  # 项目级

# 修复权限
chmod 700 ~/.claude/
chmod 600 ~/.claude/CLAUDE.md
```

## 3.3 运行时问题

### 3.3.1 高 CPU/内存占用

**问题现象：**
- Claude Code 占用大量 CPU 或内存
- 终端响应缓慢

**解决方案：**

```bash
# 检查 MCP Server 状态
> /doctor

# 压缩上下文
> /compact

# 重置上下文
> /clear

# 检查资源使用
top -pid $(pgrep -f "claude")  # macOS/Linux
Get-Process -Name node  # Windows
```

### 3.3.2 命令超时

**问题现象：**
```bash
Error: Tool execution timeout
```

**解决方案：**

```bash
# 检查网络连接
curl -I https://api.anthropic.com

# 设置代理（如果需要）
export HTTPS_PROXY="http://proxy:port"

# 清除缓存
rm -rf ~/.claude/cache/

# 重启会话
claude --clear
```

### 3.3.3 MCP Server 连接失败

**问题现象：**
```bash
Error: MCP server disconnected
```

**解决方案：**

```bash
# 检查 MCP 配置
cat .claude/mcp.json

# 测试 MCP Server 连通性
curl -I http://localhost:3000  # 检查本地 MCP 服务

# 重启 MCP Server
# 编辑 .claude/mcp.json，设置 "disabled": true 然后重新启动
```

## 3.4 性能问题

### 3.4.1 响应缓慢

**问题现象：**
- Claude 响应时间超过 30 秒
- 上下文加载缓慢

**解决方案：**

```bash
# 检查上下文大小
> /cost

# 压缩对话历史
> /compact

# 禁用不必要的 MCP Server
# 编辑 .claude/mcp.json，设置 disabled: true

# 检查网络延迟
ping api.anthropic.com
```

### 3.4.2 上下文溢出

**问题现象：**
```bash
Error: Context window exceeded
```

**解决方案：**

```bash
# 压缩上下文
> /compact

# 重置上下文
> /clear

# 分步骤处理任务
# 避免在一个会话中处理过多不相关的内容

# 精确读取文件
> 读取 src/main.ts 的第 1-50 行，而不是整个文件
```

## 3.5 调试技巧

### 3.5.1 启用调试模式

```bash
# 开启详细日志模式
export CLAUDE_DEBUG=1
claude

# 查看日志文件
tail -f ~/.claude/logs/latest.log

# 搜索错误信息
grep -i "error" ~/.claude/logs/latest.log
```

### 3.5.2 执行追踪

Claude Code 的对话窗口会记录完整的执行历史：

```bash
[Read] src/auth/jwt.ts
  --> 文件内容: ... (显示文件内容)

[Bash] npm test -- --filter auth
  --> 测试结果: 5 passed, 1 failed

[Edit] src/auth/jwt.ts
  --> 替换: line 42-45
  --> 变更预览: ...
```

### 3.5.3 系统化排查流程

```bash
# 步骤 1: 环境检查
node --version && npm --version && claude --version

# 步骤 2: 网络检查
curl -I https://api.anthropic.com

# 步骤 3: 配置检查
cat ~/.claude/CLAUDE.md 2>/dev/null && echo "---" && cat CLAUDE.md 2>/dev/null

# 步骤 4: 日志检查
tail -50 ~/.claude/logs/latest.log 2>/dev/null

# 步骤 5: 一键诊断
claude -p "/doctor"
```

---

# 第四部分：实战案例

## 4.1 快速开发 Todo 应用

### 4.1.1 项目初始化

```bash
claude --dangerously-skip-permissions

> 创建一个完整的 Todo 应用，使用：
> - React + TypeScript + Vite
> - Tailwind CSS 用于样式
> - React Query 用于状态管理
> - localStorage 用于数据存储
> - 包含添加、删除、完成、筛选功能

> 配置 ESLint 和 Prettier

> 创建 README.md 文档
```

### 4.1.2 运行与测试

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 运行测试
npm test

# 构建生产版本
npm run build
```

## 4.2 代码审查自动化

### 4.2.1 配置 GitHub Actions

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
          claude -p "审查这个 PR 的所有变更，检查以下方面：
          1. 安全性问题（硬编码密钥、SQL 注入等）
          2. 代码质量（命名、注释、复杂度）
          3. 错误处理是否完整
          4. 类型定义是否准确
          5. 是否遵循项目编码规范

          请以 Markdown 表格形式输出审查结果，包含：
          - 严重级别（High/Medium/Low）
          - 文件位置
          - 问题描述
          - 建议修改" \
            --dangerously-skip-permissions
```

### 4.2.2 本地代码审查

```bash
claude

> 审查 src/ 目录下的所有变更，重点检查：
> - 安全漏洞
> - 代码质量问题
> - 性能优化空间

> 输出审查报告
```

## 4.3 多 Claude 协作开发

### 4.3.1 架构师 Claude（计划阶段）

```bash
claude --permission-mode plan

> 为电商项目设计微服务架构，包含：
> 1. 用户服务（UserService）
> 2. 商品服务（ProductService）
> 3. 订单服务（OrderService）
> 4. 支付服务（PaymentService）

> 定义服务边界和 API 接口
> 设计数据库schema
> 制定技术选型方案
```

### 4.3.2 开发者 Claude（实现阶段）

```bash
claude --permission-mode acceptEdits

> 根据架构师的设计，实现用户服务：
> - 使用 NestJS + TypeScript
> - 实现用户 CRUD 操作
> - 添加 JWT 认证
> - 编写单元测试
```

### 4.3.3 测试 Claude（验证阶段）

```bash
claude

> 为用户服务编写集成测试：
> - 测试所有 API 端点
> - 测试边界条件
> - 测试错误处理

> 运行测试并生成测试报告
```

---

# 第五部分：最佳实践

## 5.1 CLAUDE.md 编写技巧

### 5.1.1 基本结构

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
```

### 5.1.2 高级技巧

```markdown
# 导入模块化规则
@import .claude/rules/code-style.md
@import .claude/rules/testing.md
@import .claude/rules/security.md

# 快速添加规则
# 所有 API 路由必须包含版本前缀 /api/v1/
# 组件必须导出 Props 类型定义
# 使用 logger.info 代替 console.log
```

## 5.2 权限管理

### 5.2.1 权限模式选择

```bash
# 默认模式（需要确认）
claude

# 自动模式（谨慎使用）
claude --dangerously-skip-permissions

# 计划模式（只分析）
claude --permission-mode plan

# 接受编辑但需要确认执行
claude --permission-mode acceptEdits
```

### 5.2.2 权限配置

```json
// .claude/settings.json
{
  "permissions": {
    "mode": "default",
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test*)",
      "Bash(git status)"
    ],
    "deny": [
      "Bash(rm -rf*)",
      "Bash(sudo*)"
    ]
  }
}
```

## 5.3 上下文优化

### 5.3.1 减少 Token 消耗

```bash
# 精确读取文件
> 读取 src/auth/jwt.ts 的第 20-50 行，而不是整个文件

# 避免模糊搜索
> 找到 src/components/Auth 目录下的 Login.tsx 文件，而不是 "找到登录相关的文件"

# 分步骤处理
> 第一步：分析用户服务的架构
> 第二步：实现用户认证功能
> 第三步：编写测试用例
```

### 5.3.2 会话管理

```bash
# 压缩对话历史
> /compact

# 重置上下文
> /clear

# 查看 Token 使用情况
> /cost

# 恢复会话
claude --resume
```

---

# 附录：常用命令速查表

## 基础命令

| 命令 | 功能 | 示例 |
|------|------|------|
| `claude` | 启动 Claude Code | `claude` |
| `claude -p <prompt>` | 单次任务模式 | `claude -p "解释项目架构"` |
| `claude --version` | 检查版本 | `claude --version` |
| `claude --resume` | 恢复会话 | `claude --resume` |

## 权限模式

| 模式 | 说明 |
|------|------|
| `--permission-mode default` | 默认模式（需要确认） |
| `--permission-mode acceptEdits` | 自动编辑，需确认执行 |
| `--permission-mode plan` | 只分析不执行 |
| `--dangerously-skip-permissions` | 完全自动 |

## 斜线命令

| 命令 | 功能 |
|------|------|
| `/help` | 显示帮助信息 |
| `/memory` | 编辑 CLAUDE.md |
| `/compact` | 压缩对话历史 |
| `/clear` | 清除上下文 |
| `/commit` | 生成 Commit 信息 |
| `/pr` | 创建 PR |
| `/doctor` | 诊断系统状态 |
| `/cost` | 查看 Token 使用 |

## 文件操作

| 命令 | 功能 |
|------|------|
| `Read <file>` | 读取文件 |
| `Write <file>` | 创建/重写文件 |
| `Edit <file>` | 编辑文件 |
| `Glob <pattern>` | 搜索文件 |
| `Grep <pattern>` | 搜索内容 |

---

> **报告说明**：本报告基于 2026 年 2 月的 Claude Code 版本编写，后续版本可能会有功能变化。建议结合官方文档获取最新信息。
>
> **参考资源**：
> - [官方文档](https://code.claude.com/docs)
> - [Claude 中文社区](https://claudecn.com)
> - [Skills 开发指南](https://platform.claude.com/docs)
