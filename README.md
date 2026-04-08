# cwp0.github.io

## 简介

这是我的个人博客，基于 [Hexo](https://hexo.io/) 框架搭建，使用 [Butterfly](https://butterfly.js.org/) 主题，集成 [Algolia](https://www.algolia.com/) 搜索服务。记录一些学习笔记和生活感悟。

- **博客地址**：[https://cwp0.github.io](https://cwp0.github.io)
- **分支说明**：`hexo` 分支存放博客源码，`master` 分支存放生成的静态文件

## 技术栈

- **框架**：Hexo 6.3.0
- **主题**：Butterfly
- **搜索**：Algolia Search
- **评论**：Valine
- **部署**：GitHub Pages + hexo-deployer-git

## 环境准备

在开始之前，请确保已安装以下工具：

- [Node.js](https://nodejs.org/)（建议 v14+）
- [Git](https://git-scm.com/)
- npm（随 Node.js 一起安装）

### 安装 Hexo CLI

```bash
npm install -g hexo-cli
```

## 快速开始

### 1. 克隆项目

```bash
git clone -b hexo git@github.com:cwp0/cwp0.github.io.git
cd cwp0.github.io
```

> **注意**：请切换到 `hexo` 分支，该分支包含博客源码。

### 2. 安装依赖

```bash
npm install
```

### 3. 本地预览

```bash
hexo server
```

启动后访问 [http://localhost:4000](http://localhost:4000) 即可在本地预览博客。

## 发布博客

### 1. 创建新文章

```bash
hexo new "文章标题"
```

执行后会在 `source/_posts/` 目录下生成一个 `文章标题.md` 文件，使用 Markdown 编辑即可。

文章模板的 Front-matter 示例：

```markdown
---
title: 文章标题
date: 2026-03-20 19:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类名
---

正文内容...
```

### 2. 生成静态文件

```bash
hexo generate
```

或使用简写：

```bash
hexo g
```

### 3. 部署到 GitHub Pages

```bash
hexo deploy
```

或使用简写：

```bash
hexo d
```

该命令会将生成的静态文件推送到 `master` 分支，GitHub Pages 会自动更新。

### 4. 一键生成并部署

```bash
hexo clean && hexo generate && hexo deploy
```

> **推荐**：每次部署前先执行 `hexo clean` 清除缓存和旧的静态文件，避免残留问题。

## 更新 Algolia 搜索索引

当发布新文章或修改已有文章后，需要更新 Algolia 搜索索引，使搜索结果保持最新：

```bash
hexo algolia
```

> 完整的发布流程建议：`hexo clean && hexo g && hexo algolia && hexo d`

## 常用命令速查

| 命令 | 说明 |
| --- | --- |
| `hexo new "标题"` | 创建新文章 |
| `hexo new draft "标题"` | 创建草稿 |
| `hexo new page "页面名"` | 创建新页面 |
| `hexo server` | 启动本地预览服务（默认端口 4000） |
| `hexo generate` | 生成静态文件 |
| `hexo deploy` | 部署到远程仓库 |
| `hexo clean` | 清除缓存和已生成的静态文件 |
| `hexo algolia` | 更新 Algolia 搜索索引 |

## 目录结构

```
.
├── _config.yml              # Hexo 主配置文件
├── _config.butterfly.yml    # Butterfly 主题配置文件
├── package.json             # 项目依赖
├── scaffolds/               # 文章模板
│   ├── draft.md             # 草稿模板
│   ├── page.md              # 页面模板
│   └── post.md              # 文章模板
└── source/                  # 资源文件夹
    ├── _data/               # 数据文件（友链等）
    ├── _posts/              # 博客文章（Markdown）
    ├── about/               # 关于页面
    ├── categories/          # 分类页面
    ├── link/                # 友链页面
    ├── movies/              # 电影页面
    ├── music/               # 音乐页面
    └── tags/                # 标签页面
```

## 保存博客源码

部署命令只会将生成的静态文件推送到 `master` 分支。**博客源码需要手动推送到 `hexo` 分支**：

```bash
git add .
git commit -m "更新博客源码"
git push origin hexo
```

> **重要**：养成每次修改后同时推送源码的习惯，防止源码丢失。
