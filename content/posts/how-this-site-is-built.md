---
title: "Hugo + PaperMod + GitHub Pages 本站搭建全记录"
date: 2026-10-11T15:10:00+08:00
lastmod: 2026-10-11T15:10:00+08:00
draft: false
author: "ikaodian"
summary: "从零把这个站点搭起来：装 Hugo、套 PaperMod 主题、写内容、配 GitHub Actions 自动发布，把踩到的坑一并记下来。"
tags: ["Hugo", "GitHub Pages", "教程"]
categories: ["工具与折腾"]
showToc: true
TocOpen: true
---

## 结论先行

整套方案是：**Hugo 生成静态页 → Git 仓库存源码 → GitHub Actions 构建并发布到 Pages**。免费、无服务器、访问速度取决于 Pages，够个人用。

需要的东西就三样：Hugo（扩展版）、一个主题、一个 GitHub 仓库。

## 一、安装 Hugo

Windows 上推荐直接下 release 里的 zip，解压即用，不依赖管理员权限：

```bash
# 下载扩展版（extended）—— 用了 SCSS 的主题必须要它
curl -L -o hugo.zip \
  https://github.com/gohugoio/hugo/releases/download/v0.167.0/hugo_extended_0.167.0_windows-amd64.zip

# 解压后把 hugo.exe 所在目录加入 PATH
hugo version
# hugo v0.167.0+extended windows/amd64
```

> 如果 winget 下载卡住或报 `InternetOpenUrl() failed`，直接换 curl 下载 zip，绕开问题。

## 二、初始化站点

```bash
hugo new site my-blog --force
cd my-blog
git init
```

目录职责记一下，后面天天和它们打交道：

```
content/    文章骨架（Markdown）
layouts/    覆盖主题的模板
assets/     CSS/JS，会被打包压缩
static/     原样拷贝的文件（图片、favicon）
i18n/       多语言文案
themes/     主题
hugo.toml   站点配置
```

## 三、装主题

用 git submodule，主题更新和站点内容互不干扰：

```bash
git submodule add --depth=1 \
  https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

然后在 `hugo.toml` 里声明 `theme = "PaperMod"`。

PaperMod 的中文语言包叫 `zh.yaml`，而站点 `languageCode = "zh-cn"` 会去找 `zh-cn.yaml`。稳妥做法是复制一份：

```bash
cp themes/PaperMod/i18n/zh.yaml i18n/zh-cn.yaml
```

## 四、配置要点

配置项里容易踩的几个地方：

| 配置 | 说明 |
| --- | --- |
| `hasCJKLanguage = true` | 中文摘要按字符数截断，不设会按"词"算导致摘要超长 |
| `[pagination] pagerSize` | Hugo 0.128 起 `paginate` 已弃用，用新写法 |
| `outputs.home` 加 `"JSON"` | PaperMod 的站内搜索依赖 JSON 索引 |
| `markup.goldmark.renderer.unsafe = true` | 允许正文里写原生 HTML |
| `summaryLength` | 首页摘要长度，中文站建议 100～150 |

菜单直接写在配置里：

```toml
[[menu.main]]
  name = "归档"
  url = "archives/"
  weight = 10
```

## 五、写文章

```bash
hugo new content posts/my-first-post.md
```

生成的草稿默认 `draft: true`，本地预览加 `-D`，正式发布记得改成 `false`。

正文里可以放心用 Markdown 扩展语法，表格、引用、代码块都能正常渲染：

```python
def fib(n: int) -> int:
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

## 六、本地预览

```bash
hugo server -D --bind 127.0.0.1 --port 1313
```

打开 `http://127.0.0.1:1313`，改文件会热重载。

发布前跑一次完整构建，确认没有报错：

```bash
hugo --gc --minify
```

## 七、自动发布到 GitHub Pages

在 `.github/workflows/hugo.yml` 里配置：检出时拉子模块 → 装 Hugo → 构建 → 上传 artifact → 部署到 Pages。

```yaml
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v3
  with:
    hugo-version: "0.167.0"
    extended: true

- name: Build
  run: hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"
```

关键点：**检出仓库时必须加 `submodules: recursive`**，否则 `themes/PaperMod` 是空目录，构建会直接失败。

另外，Pages 的发布源在仓库 `Settings → Pages` 里只能选一个：要么"分支"，要么"GitHub Actions"。两套发布流程同时开着会互相覆盖。

## 踩过的坑清单

1. `hugo: command not found` —— 安装目录没进 PATH，重开终端。
2. 构建报 `module "PaperMod" not found` —— 主题没拉取，跑 `git submodule update --init --recursive`。
3. 页面样式全丢 —— `baseURL` 写错，生成的资源链接指向了错误域名。
4. 首页摘要长到离谱 —— 忘了开 `hasCJKLanguage`。
5. Actions 里构建成功但页面 404 —— Pages 源没切到 GitHub Actions。

---

至此站点就跑起来了。剩下的都是内容活：写得慢一点、准一点，比写得多更重要。
