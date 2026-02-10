# 证据表: tmux vs byobu

## 活跃度筛查结果

### tmux
- **GitHub 星标**: ~41.6k stars
- **最近更新**: 2025年12月 (持续活跃)
- **发布周期**: 每6个月一次新版本 (5月和10月,与 OpenBSD 同步)
- **提交活动**: 持续活跃,有多个贡献者
- **Fork 数**: 2.2k
- **结论**: ✅ 活跃项目,核心维护良好

### byobu
- **GitHub 星标**: ~1.3k stars
- **最近更新**: 2025年8月 (v6.13)
- **最近提交**: 2025年3月 (v6.12 - 添加 git branch 到 bash ps1 prompt)
- **结论**: ✅ 活跃项目,持续维护

**两者都通过活跃度筛查**

## 源质量评分

| Source ID | Title | Publisher | Date | URL | Quality Tier | Notes |
|-----------|-------|-----------|------|-----|--------------|-------|
| S1 | tmux GitHub | GitHub/tmux org | 2025 | https://github.com/tmux/tmux | A | 官方仓库,权威来源 |
| S2 | byobu GitHub | GitHub/dustinkirkland | 2025 | https://github.com/dustinkirkland/byobu | A | 官方仓库,权威来源 |
| S3 | tmux vs Byobu - Slant | Slant Community | 2025 | https://www.slant.co/versus/11858/11861/~tmux_vs_byobu | B | 社区投票和评论,有用户洞察 |
| S4 | tmux/byobu - Project PANOPTES | Project PANOPTES | 2023 | https://www.projectpanoptes.org/build/software/appendix/tmux-byobu | B | 项目文档,实际使用经验 |
| S5 | Byobu official site | byobu.org | 2025 | https://www.byobu.org/ | A | 官方网站 |
| S6 | Ubuntu Server docs - Byobu | Ubuntu/Canonical | 2025 | https://ubuntu.com/server/docs/byobu | A | 官方发行版文档 |
| S7 | DigitalOcean Tutorial | DigitalOcean | 2016 | https://www.digitalocean.com/community/tutorials/how-to-install-and-use-byobu-for-terminal-management-on-ubuntu-16-04 | C | 教程,但版本较旧 |
| S8 | Byobu Wikipedia | Wikipedia | 2025 | https://en.wikipedia.org/wiki/Byobu_(software) | B | 综合参考,社区编辑 |
| S9 | Introduction to Byobu | Better Programming/Medium | 2022 | https://betterprogramming.pub/introduction-to-byobu-a-window-manager-and-terminal-multiplexer-d8ef0cc278d9 | B | 技术博客,详细教程 |
| S10 | Byobu with Flox | Flox | 2024 | https://flox.dev/popular-packages/get-perfectly-good-terminal-multiplexing-with-byobu-and-flox/ | B | 技术文章,实际使用经验 |
| S11 | Multiplex - Linux Magazine | Linux Magazine | 2015 | https://www.linux-magazine.com/index.php/Issues/2015/175/Command-Line-Byobu | B | 技术杂志,深入分析 |
| S12 | SuperUser Q&A | Stack Exchange | 2012 | https://superuser.com/questions/423310/byobu-vs-gnu-screen-vs-tmux-usefulness-and-transferability-of-skills | B | 社区讨论,高质量回答 |
| S13 | UPC Persistent sessions | UPC | 2025 | https://tsc.upc.edu/en/it-services/computing-services/installed-software-instructions/persistent-sesions-with-tmux-and-byobu | B | 大学IT文档 |
| S14 | Linux.org forum | Linux.org | 2014 | https://www.linux.org/threads/screen-vs-tmux-vs-byobu-vs.10350/ | C | 论坛讨论,较旧 |
| S15 | 知乎讨论 | 知乎 | 2025 | https://www.zhihu.com/question/347428244 | B | 中文社区讨论 (访问失败) |
| S16 | 博客园文章 | 博客园 | 2020 | https://www.cnblogs.com/dgp-zjz/p/14016284.html | B | 中文技术博客,功能介绍 |

## 核心发现映射

### 发现 1: byobu 是 tmux 的封装器
- **证据**: S1, S2, S3, S4, S5, S6
- **置信度**: 极高
- **引用**: "Byobu is actually just a wrapper around tmux" (S3, S4)

### 发现 2: 命令键差异
- **证据**: S3, S4, S9
- **tmux**: Ctrl-b
- **byobu**: Ctrl-a
- **置信度**: 高

### 发现 3: 易用性对比
- **证据**: S3, S9, S10, S11
- **byobu**: F1-F12 快捷键,内置帮助,开箱即用
- **tmux**: 需要配置,学习曲线陡峭
- **置信度**: 高

### 发现 4: 性能开销
- **证据**: S3, S10
- **核心问题**: byobu 状态栏每秒执行外部脚本
- **影响**: 大量进程 fork 和脚本解释
- **置信度**: 中等 (需要实际性能测试验证)

### 发现 5: 平台可用性
- **证据**: S3, S6
- **byobu**: Ubuntu Server 默认安装
- **tmux**: 跨平台更广泛
- **置信度**: 高

### 发现 6: 社区推荐
- **证据**: S3
- **Slant 排名**: tmux 第1, byobu 第3
- **置信度**: 中等 (基于社区投票)

### 发现 7: 活跃度
- **证据**: S1, S2 (GitHub API)
- **tmux**: 41.6k stars, 持续活跃
- **byobu**: 1.3k stars, 持续维护
- **置信度**: 极高

### 发现 8: 状态监控功能
- **证据**: S5, S9, S10, S11
- **byobu**: 丰富的系统监控 (CPU/内存/网络/更新等)
- **tmux**: 基础状态栏,需配置扩展
- **置信度**: 高

## 证据覆盖度评估

- ✅ 核心定位和关系: 充分
- ✅ 功能差异: 充分
- ✅ 易用性对比: 充分
- ⚠️ 性能对比: 中等 (缺少定量基准测试)
- ✅ 平台可用性: 充分
- ✅ 社区活跃度: 充分
- ✅ 使用场景: 充分
- ⚠️ 配置和定制细节: 中等 (可扩展,但当前证据足够概览报告)
