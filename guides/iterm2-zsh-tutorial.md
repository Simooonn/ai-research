# iTerm2 + Oh My Zsh + Powerlevel10k 使用教程

## 📦 已安装组件

### 核心框架
- ✅ **Oh My Zsh** - Zsh 配置框架
- ✅ **Powerlevel10k** - 美观强大的主题

### 插件
- ✅ **zsh-autosuggestions** - 历史命令自动建议
- ✅ **zsh-syntax-highlighting** - 命令语法高亮
- ✅ **fzf** - 模糊查找工具 (v0.67.0)
- ✅ **autojump** - 智能目录跳转 (v22.5.3)

### 其他内置插件
- **git** - Git 命令别名
- **docker** - Docker 命令补全
- **kubectl** - Kubernetes 命令补全
- **npm** - npm 命令补全

---

## 🚀 快速开始

### 重新加载配置
```bash
source ~/.zshrc
```
或者重启终端

---

## 🎨 Powerlevel10k 主题配置

### 首次配置
- 首次加载 Zsh 时会自动启动配置向导
- 按照提示选择：
  - 字体样式（推荐 MesloLGS NF）
  - 提示符布局（建议选择 Rainbow 或 Lean）
  - 图标显示（建议启用）
  - Git 状态显示（建议启用）
  - 时间显示（根据喜好）
  - 命令执行时间（建议启用）

### 重新配置
```bash
p10k configure
```

### 配置文件位置
```bash
~/.p10k.zsh
```

### 临时切换其他主题
编辑 `~/.zshrc`，找到 `ZSH_THEME` 行：
```bash
# 默认
ZSH_THEME="powerlevel10k/powerlevel10k"

# 切换到其他主题（如 agnoster）
ZSH_THEME="agnoster"
```

---

## 🔍 fzf - 模糊查找工具

### 快捷键

| 快捷键 | 功能 | 说明 |
|--------|------|------|
| `Ctrl + R` | 搜索命令历史 | 交互式搜索并执行历史命令 |
| `Ctrl + T` | 搜索文件 | 在当前目录递归搜索文件 |
| `Alt + C` | 切换目录 | 搜索并切换到子目录 |

### 使用示例

#### 1. 搜索历史命令
```bash
# 按 Ctrl + R，然后输入关键词
# 例如：输入 "git" 搜索所有 git 相关命令
```

#### 2. 搜索文件
```bash
# 按 Ctrl + T
# 输入文件名关键词，选择后自动粘贴到命令行
vim <Ctrl+T>  # 然后搜索文件
```

#### 3. 切换目录
```bash
# 按 Alt + C
# 搜索目录名，选择后自动切换
```

### 命令行使用

```bash
# 在任何命令后使用 **<Tab> 触发 fzf
vim **<Tab>        # 搜索文件
cd **<Tab>         # 搜索目录
kill -9 **<Tab>    # 搜索进程

# 管道使用
cat file.txt | fzf  # 在文本内容中搜索
```

---

## 💡 zsh-autosuggestions - 命令自动建议

### 功能说明
- 根据命令历史自动建议
- 建议以灰色文字显示在光标后

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `→` (右箭头) | 接受整个建议 |
| `Ctrl + →` | 接受一个词 |
| `End` | 接受整个建议（另一种方式）|
| `Esc` | 取消建议 |

### 使用示例
```bash
# 输入: git
# 显示: git push origin main (灰色)
# 按 → 接受: git push origin main

# 输入: cd ~/Doc
# 显示: cd ~/Documents (灰色)
# 按 → 接受
```

---

## 🎨 zsh-syntax-highlighting - 语法高亮

### 颜色含义

| 颜色 | 含义 | 示例 |
|------|------|------|
| 🟢 绿色 | 有效命令 | `ls`, `git`, `npm` |
| 🔴 红色 | 无效命令 | `lss`, `gti` (拼写错误) |
| 🔵 蓝色 | 路径/目录 | `~/Documents`, `/etc` |
| 🟡 黄色 | 字符串 | `"hello"`, `'world'` |
| 🟣 紫色 | 选项/参数 | `-l`, `--help` |

### 特殊标记
- **下划线** - 文件/目录存在
- **普通文本** - 文件/目录不存在

---

## 🚀 autojump - 智能目录跳转

### 基本使用

```bash
# 1. 首先访问一些目录，让 autojump 建立索引
cd ~/Documents
cd ~/Projects/my-app
cd ~/Downloads

# 2. 之后可以使用 j 命令快速跳转
j doc          # 跳转到 ~/Documents
j proj         # 跳转到 ~/Projects
j my           # 跳转到 ~/Projects/my-app
```

### 常用命令

```bash
# 跳转到包含关键词的目录
j <关键词>

# 跳转到子目录
jc <关键词>     # 只在当前目录的子目录中搜索

# 在文件管理器中打开目录
jo <关键词>     # macOS 使用 Finder 打开

# 查看目录权重统计
j -s           # 显示所有被记录的目录及权重

# 增加当前目录权重
j -i [权重]    # 手动增加权重

# 减少目录权重
j -d [权重]    # 手动减少权重

# 清理无效路径
j --purge      # 删除不存在的目录
```

### 使用技巧

1. **模糊匹配**
   ```bash
   # 访问过 ~/Projects/my-awesome-app
   j awe          # 可以匹配到
   j my awe       # 多个关键词也可以
   ```

2. **权重系统**
   - 访问频率越高，权重越大
   - 最近访问的目录权重更高
   - 自动清理很久未访问的目录

3. **结合其他命令**
   ```bash
   # 跳转并执行命令
   j proj && git status

   # 在目标目录执行命令
   cd $(autojump proj) && ls -la
   ```

---

## 🔧 Git 插件别名

Oh My Zsh 的 git 插件提供了大量便捷别名：

### 常用别名

```bash
# 状态和信息
g        → git
gst      → git status
gl       → git pull
gp       → git push
glog     → git log --oneline --decorate --graph

# 分支操作
gb       → git branch
gba      → git branch -a
gco      → git checkout
gcb      → git checkout -b
gm       → git merge

# 提交操作
ga       → git add
gaa      → git add --all
gc       → git commit -v
gc!      → git commit -v --amend
gcmsg    → git commit -m

# 暂存操作
gsta     → git stash
gstp     → git stash pop
gstl     → git stash list

# 差异和日志
gd       → git diff
gdca     → git diff --cached
glg      → git log --stat
glgp     → git log --stat -p
```

### 查看所有别名
```bash
alias | grep git
```

---

## ⚙️ 配置文件

### 主要配置文件位置

```bash
~/.zshrc                                    # Zsh 主配置文件
~/.p10k.zsh                                 # Powerlevel10k 配置
~/.fzf.zsh                                  # fzf 配置
~/.oh-my-zsh/                               # Oh My Zsh 目录
~/.oh-my-zsh/custom/plugins/                # 自定义插件目录
~/.oh-my-zsh/custom/themes/                 # 自定义主题目录
```

### 编辑配置文件

```bash
# 快速编辑 .zshrc
vim ~/.zshrc

# 或创建别名（添加到 ~/.zshrc）
alias zshconfig="vim ~/.zshrc"
alias zshreload="source ~/.zshrc"
```

### 修改插件配置

编辑 `~/.zshrc`，找到 `plugins=()` 部分：

```bash
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  autojump
  fzf
  docker
  kubectl
  npm
  # 添加其他插件...
)
```

---

## 🎯 高级技巧

### 1. 自定义别名

在 `~/.zshrc` 末尾添加：

```bash
# 常用命令别名
alias ll='ls -lah'
alias cls='clear'
alias ..='cd ..'
alias ...='cd ../..'

# Git 快捷操作
alias gs='git status'
alias gp='git push'
alias gl='git pull'

# 快速目录跳转
alias proj='cd ~/Projects'
alias doc='cd ~/Documents'

# 编辑器
alias vi='vim'
alias code='open -a "Visual Studio Code"'
```

### 2. 自定义函数

```bash
# 创建目录并进入
mkcd() {
  mkdir -p "$1" && cd "$1"
}

# 查找并杀死进程
kp() {
  ps aux | grep "$1" | grep -v grep | awk '{print $2}' | xargs kill -9
}

# 快速备份文件
backup() {
  cp "$1"{,.bak}
}
```

### 3. 环境变量

```bash
# 添加到 PATH
export PATH="$HOME/bin:$PATH"

# 设置默认编辑器
export EDITOR='vim'

# 代理设置（如需要）
export http_proxy='http://127.0.0.1:7890'
export https_proxy='http://127.0.0.1:7890'
```

### 4. fzf 高级配置

在 `~/.zshrc` 中添加：

```bash
# fzf 默认选项
export FZF_DEFAULT_OPTS='
  --height 40%
  --layout=reverse
  --border
  --preview "cat {}"
'

# 使用 fd 替代 find（如果已安装）
export FZF_DEFAULT_COMMAND='fd --type f --hidden --follow --exclude .git'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"

# 目录搜索使用 fd
export FZF_ALT_C_COMMAND='fd --type d --hidden --follow --exclude .git'
```

---

## 🔍 常见问题

### 1. 终端字体显示异常

**问题：** Powerlevel10k 图标显示为乱码

**解决：**
1. 下载并安装 Nerd Font：
   ```bash
   brew tap homebrew/cask-fonts
   brew install --cask font-meslo-lg-nerd-font
   ```
2. 在 iTerm2 中设置字体：
   - Preferences → Profiles → Text → Font
   - 选择 "MesloLGS NF"

### 2. 命令补全不工作

**解决：**
```bash
# 重建补全缓存
rm -f ~/.zcompdump*
autoload -U compinit && compinit
```

### 3. autojump 跳转不准确

**解决：**
```bash
# 清理无效路径
j --purge

# 查看权重并手动调整
j -s
j -i 100  # 增加当前目录权重
```

### 4. 插件加载慢

**解决：**
```bash
# 分析启动时间
time zsh -i -c exit

# 禁用不需要的插件
# 编辑 ~/.zshrc，删除 plugins=() 中不需要的项
```

### 5. fzf 快捷键不生效

**解决：**
```bash
# 确保 fzf 已正确初始化
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

# 重新安装 key bindings
~/.fzf/install --key-bindings --completion --no-update-rc
```

---

## 📚 更多资源

### 官方文档
- [Oh My Zsh](https://ohmyz.sh/)
- [Powerlevel10k](https://github.com/romkatv/powerlevel10k)
- [fzf](https://github.com/junegunn/fzf)
- [autojump](https://github.com/wting/autojump)
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

### 推荐插件
```bash
# 在 Oh My Zsh 插件目录查看可用插件
ls ~/.oh-my-zsh/plugins/

# 常用推荐：
# - web-search: 终端中搜索网页
# - extract: 智能解压缩
# - sudo: 按 Esc 两次在命令前添加 sudo
# - colored-man-pages: 彩色 man 文档
# - z: 类似 autojump 的目录跳转
```

### 卸载

如果需要卸载：

```bash
# 卸载 Oh My Zsh
uninstall_oh_my_zsh

# 卸载 fzf
~/.fzf/uninstall

# 卸载 autojump
brew uninstall autojump
```

---

## 📝 快速参考卡

### 必记快捷键
```
Ctrl + R     - 搜索历史命令 (fzf)
Ctrl + T     - 搜索文件 (fzf)
Alt + C      - 切换目录 (fzf)
→            - 接受命令建议
j <关键词>    - 跳转目录 (autojump)
p10k configure - 配置主题
```

### 常用命令
```bash
source ~/.zshrc    # 重新加载配置
j -s              # 查看 autojump 统计
alias | grep git  # 查看 git 别名
gst               # git status
gaa               # git add --all
gcmsg "msg"       # git commit -m "msg"
```

---

**最后更新：** 2026-02-10
**版本：** 1.0
