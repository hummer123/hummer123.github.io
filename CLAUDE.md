# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概览

基于 **Hugo** 的个人 Notebook（博客）静态站点，使用 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题。

- 部署目标：`https://hummer123.github.io/`（GitHub Pages）
- 默认语言：中文（`zh-cn`），启用 CJK 字数统计
- 当前 Hugo 工具链：`hugo v0.163.3+extended+withdeploy`（PaperMod 要求 ≥ v0.146.0）
- 主题来源：`themes/PaperMod` 是 git submodule（详见 `.gitmodules`），不要直接修改该目录

## 常用命令

所有命令均在仓库根目录执行。

| 场景 | 命令 |
| --- | --- |
| 本地预览（含 draft） | `hugo server -D`（默认监听 `http://localhost:1313`） |
| 仅预览已发布 | `hugo server` |
| 构建静态站点到 `public/` | `hugo` 或 `hugo --minify` |
| 构建并预演部署（带 deploy target） | `hugo --gc --minify` |
| 新建文章（按 `archetypes/default.md` 生成 front matter） | `hugo new posts/<slug>.md` |
| 新建 about / archives 等独立页 | `hugo new about.md` / `hugo new archives.md` |
| 更新主题子模块 | `git submodule update --remote --merge` |
| 首次克隆后初始化子模块 | `git submodule update --init --recursive` |

构建配置由 `hugo.toml` 唯一管理，没有额外的 Makefile / package.json / CI 脚本。

## 目录结构与约定

```
.
├── archetypes/default.md   # 新建内容时的 front matter 模板（生成 draft=true）
├── assets/                 # Hugo 资源管线（SCSS/JS），目前空
├── content/                # Markdown 内容源（目前空，需在此创建 posts/、about.md 等）
├── data/                   # 自定义数据文件（JSON/YAML/TOML），目前空
├── hugo.toml               # 全站唯一配置
├── i18n/                   # 多语言字符串覆盖（目前空）
├── layouts/                # 模板覆盖（目前空；按需覆盖 PaperMod 模板）
├── public/                 # `hugo` 构建产物（已在 git 中提交，见下）
├── static/                 # 原样拷贝到站点根的静态文件（favicon 等），目前空
└── themes/PaperMod/        # 主题（git submodule，不要直接改）
```

**内容组织**：
- 文章统一放在 `content/posts/`，归档（archives）由 PaperMod 自动生成，无需手写。
- 顶部菜单三项（`posts` / `archives` / `about`）已在 `hugo.toml` 定义，对应 `/posts/`、`/archives/`、`/about/`。需要新增菜单项时改 `[menu.main]`。
- `archetypes/default.md` 默认带 `draft = true`；发布前需改为 `false`，否则 `hugo`（非 `-D`）不会输出。

## 关键配置点（`hugo.toml`）

修改前请先确认影响面；以下开关常被改动：

- 评论系统：`params.comments = false`（默认关闭，开启时需配置 Giscus 相关参数，PaperMod 文档有说明）
- 主题色：`params.defaultTheme = "auto"`（跟随系统），可选 `"light"` / `"dark"`
- 代码高亮：`[markup.highlight]` 的 `style = "monokai"`、`lineNos = true`、`ShowCodeCopyButtons = true`
- TOC 深度：`[markup.tableOfContents].endLevel = 3`
- 首页模式：默认"最新文章列表"；启用"profile mode"需要把 `[params.profileMode]` 块取消注释
- Markdown 渲染：`[markup.goldmark.renderer].unsafe = true`（允许内联 HTML，XSS 风险自负）

## 关于 `public/` 与 `.gitignore`

仓库当前**没有** `.gitignore`，但 `public/`（构建产物）已被加入 git 提交。这是历史遗留状态，二选一：

- 推荐方案：新建 `.gitignore` 忽略 `public/`、`resources/`、`.hugo_build.lock`，让 CI/部署流程负责构建。
- 现状兼容：保持把 `public/` 入库（适合直接 push 到 `gh-pages` 分支的简单部署）。

修改此项前请先与用户确认，因为这会改变 git 历史和部署链路。

## 主题覆盖

需要调整主题样式/模板时，**优先在仓库根目录的 `layouts/` 和 `assets/` 下做覆盖**，不要直接修改 `themes/PaperMod/`（submodule 改动会被丢弃）。PaperMod 的 wiki 写明了覆盖路径与变量名。

## 测试与验证

本项目**没有自动化测试**（静态博客，没有单测/集成测试可言）。改动后的最小验证清单：

1. `hugo` 能完整构建无报错（关注 deprecated 警告与未定义变量）
2. `hugo server -D` 浏览器预览，确认排版、目录、代码块、菜单、暗色模式
3. 中文文章字数、摘要（`hasCJKLanguage = true`）显示正常
4. 涉及 RSS / sitemap 的改动检查 `public/index.xml` 和 `public/sitemap.xml`