# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在本仓库中工作时提供指引。

## 这个仓库是什么

这是一个 **GitHub 个人主页 README 仓库**（`daitcl/daitcl`，即与 GitHub 用户名同名的特殊仓库）。它不是一个软件项目：没有源代码、构建系统、测试套件或 Lint 工具。该仓库的唯一用途是渲染 https://github.com/daitcl 上展示的个人主页。

- `README.md` — 主页内容，使用中文编写，会直接渲染为 GitHub 个人主页页面。包含作者 CSDN 博客、个人网站（daitcc.top）、爱发电赞助、邮箱、微信公众号等链接。
- `profile/stats.svg`、`profile/top-langs.svg` — README 通过相对路径（`./profile/stats.svg`）引用的 GitHub 统计卡片。
- `.github/workflows/` — 仓库中唯一有实际意义的"代码"，两个 GitHub Actions 负责主页的自动化。

## GitHub Actions（需要了解的自动化）

### `.github/workflows/stats.yml` — 重新生成统计卡片
- 定时每天 UTC 03:00 运行，也支持 `workflow_dispatch` 手动触发。
- 使用 `readme-tools/github-readme-stats-action@v1` 重新生成 `profile/stats.svg` 和 `profile/top-langs.svg`。
- 自动提交这些 SVG（提交信息为 "Update README cards"）并推送回仓库——因此 `profile/*.svg` 可能在 main 分支上自动变化，无需本地修改。请将这两个文件视为**生成产物**，不要手动编辑。

### `.github/workflows/sync.yml` — 镜像推送到 Gitee 和 GitCode
- 在**任意分支上的每次 push** 时触发（另支持 `workflow_dispatch`）。
- 如果 Gitee / GitCode 上不存在同名仓库，会通过各自的 V5 REST API 自动创建公开仓库，然后 **强制推送** 分支和标签到两个镜像。
- 需要在 GitHub 仓库中配置 `GITEE_TOKEN` 和 `GITCODE_TOKEN` 两个 Secret。
- 含义：向本 GitHub 仓库推送任意提交都会立即强制镜像到 Gitee / GitCode。只有尚未推送的本地提交是安全的；请谨慎推送临时分支或改写历史。

## 编辑工作流

- 内容变更只涉及 Markdown/SVG：编辑 `README.md`，提交并推送即可——`sync.yml` 会自动处理 Gitee/GitCode 的镜像同步。
- 要刷新统计卡片时，通过 GitHub 界面触发工作流（Actions → Update README cards → Run workflow）。没有本地的工具可以重新生成这些 SVG。

## 约定

- README 和提交信息使用**中文**。
- 该仓库曾在 public/private 之间切换过；README 中包含爱发电创作者认证声明，要求个人主页保持公开。
