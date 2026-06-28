# yanpeng's Notebook

个人博客 / 笔记站点，源码基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题。

线上地址：<https://hummer123.github.io/>

---

## 环境要求

- Hugo **`v0.146.0` 及以上**（推荐使用 `extended` 版本，且启用 `withdeploy`，便于本地直接推送到 GitHub Pages）
- Git（PaperMod 作为 git submodule 引入）

检查本地环境：

```sh
hugo version
# 期望输出形如：hugo v0.163.3+extended+withdeploy darwin/arm64
```

---

## 快速开始

首次克隆后**必须**初始化子模块（PaperMod 在 `themes/PaperMod` 下）：

```sh
git clone <repo-url> notebook
cd notebook
git submodule update --init --recursive
```

本地预览（包含 draft）：

```sh
hugo server -D
# 浏览器打开 http://localhost:1313
```

---

## 写一篇新文章

通过 archetype 自动生成 front matter（默认带 `draft = true`）：

```sh
hugo new posts/my-first-post.md
```

写完后把 front matter 中的 `draft` 改为 `false`（或删除该字段），再次运行 `hugo server`（不带 `-D`）可验证它会出现在正式列表里。

---

## 构建与部署

生成静态站点到 `public/`：

```sh
hugo --gc --minify
```

部署到 GitHub Pages（User Page，对应仓库名 `hummer123.github.io`）的常用方式有两种：

| 方式 | 说明 |
| --- | --- |
| **GitHub Actions 推送到 `gh-pages`**（推荐） | 推代码 → CI 跑 `hugo` → 发布到 `gh-pages` 分支。配置见 `.github/workflows/`。 |
| **`hugo deploy`**（需要 `extended+withdeploy` 版本） | 在 `hugo.toml` 中配置 `[deployment]` 段后，本地一条命令直推。 |

> 注：`public/` 已加入 `.gitignore`，**不要**手动把它提交到 `main` 分支。

---

## 目录速览

| 路径 | 用途 |
| --- | --- |
| `content/posts/` | 博客文章 Markdown 源 |
| `content/about.md` | "关于" 页 |
| `archetypes/default.md` | `hugo new` 时使用的 front matter 模板 |
| `hugo.toml` | 站点唯一配置（菜单、主题参数、Markdown 渲染等） |
| `layouts/` / `assets/` | 主题/样式覆盖（**优先在这里改**，不要动 submodule） |
| `themes/PaperMod/` | 主题源码（git submodule，不要直接修改） |
| `static/` | 原样拷贝到站点根的静态文件（favicon、robots.txt 等） |

---

## 主题定制

需要修改主题模板或样式时，在仓库根目录的 `layouts/` 或 `assets/` 下**新建同名文件**即可覆盖 PaperMod 的对应文件。

**不要直接修改 `themes/PaperMod/`**——那是 git submodule，任何改动在下次 `git submodule update` 时会被丢弃。

更新主题到上游最新版本：

```sh
git submodule update --remote --merge
```

---

## License

未指定。如需开源使用请补充 LICENSE 文件。