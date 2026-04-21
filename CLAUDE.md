# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目介绍

这是一个基于 [VitePress](https://vitepress.dev/) 构建的个人技术博客，使用 [vitepress-theme-curve](https://github.com/imsyy/vitepress-theme-curve) 主题。内容以 Markdown 格式编写。

## 目录结构

```
├── .vitepress/            # VitePress 配置目录
│   ├── config.mjs        # 主配置文件
│   ├── init.mjs          # 初始化主题配置
│   ├── cache/            # 构建缓存（已在 .gitignore 中）
│   ├── dist/             # 构建输出（已在 .gitignore 中）
│   └── theme/            # 主题源码
│       ├── assets/       # 主题资源
│       │   └── themeConfig.mjs  # 主题配置（站点信息、导航、侧边栏等）
│       ├── components/   # Vue 组件
│       ├── store/        # Pinia 状态管理
│       ├── style/        # 样式文件
│       ├── utils/        # 工具函数
│       └── views/        # 页面视图
├── .github/
│   └── workflows/
│       └── ci.yml        # GitHub Actions 自动部署配置
├── page/                 # 分页模块
├── pages/                # 独立页面
│   ├── about.md          # 关于页
│   ├── archives.md       # 归档页
│   ├── categories.md     # 分类页
│   ├── link.md           # 友链页
│   ├── project.md        # 项目页
│   └── tags.md           # 标签页
├── posts/                # 文章目录，按日期分类存放
├── public/               # 静态资源（图片、字体、favicon 等）
│   └── images/logo/      # Logo 和 favicon
├── index.md              # 首页
├── package.json          # 项目依赖
└── vercel.json           # Vercel 部署配置
```

## 常用命令

### 安装依赖
```bash
npm install
# 或使用 pnpm (推荐)
pnpm install
```

### 本地开发
```bash
npm run dev
# 或
pnpm dev
```
启动本地开发服务器，访问 `http://localhost:9877` 预览博客。

### 构建生产版本
```bash
npm run build
# 或
pnpm build
```
构建输出到 `.vitepress/dist` 目录。

### 本地预览构建结果
```bash
npm run preview
# 或
pnpm preview
```

### 代码格式化
```bash
npm run format
npm run lint
```

## 配置说明

### 主题配置

主题的所有配置都在 `.vitepress/theme/assets/themeConfig.mjs`，主要包括：

- **siteMeta**: 站点基本信息（标题、描述、logo、地址、作者信息）
- **nav**: 顶部导航栏菜单
- **navMore**: 左侧下拉更多菜单
- **footer.social**: 页脚社交链接
- **aside**: 侧边栏组件配置（hello 欢迎语、toc 目录、tags 标签、倒计时、站点数据）
- **comment**: 评论系统配置（支持 artalk / twikoo）
- **search**: Algolia 搜索配置
- **music**: 音乐播放器配置
- **friends**: 友链配置

### 添加文章

在 `posts/` 目录按照日期分类创建 Markdown 文件即可，frontmatter 配置示例：

```md
---
title: 文章标题
date: 2024-10-10
categories: 分类
tags: [标签1, 标签2]
cover: https://example.com/cover.jpg
---

文章内容...
```

### 添加页面

在 `pages/` 目录创建 Markdown 文件即可，文件路径即为访问 URL。

## 部署

### GitHub Pages 自动部署

项目已配置 GitHub Actions，当推送代码到 `main` 分支时，自动构建并部署到 `gh-page` 分支。

**配置步骤**：
1. 在 GitHub 创建 Personal Access Token (PAT)，权限范围勾选 `repo`
2. 在仓库 → Settings → Secrets and variables → Actions 添加 **Name**: `ACCESS_TOKEN`，**Value**: 你的 PAT
3. 在仓库 → Settings → Pages → Build and deployment → Source 选择 **Branch**: `gh-page`，Folder: `/root`，保存
4. 推送代码到 main 分支触发构建，几分钟后即可在 `https://bingxueyizu.github.io/` 访问

### Vercel 部署

项目已包含 `vercel.json`，直接导入仓库到 Vercel 即可自动部署。

## 架构说明

- 基于 VitePress 1.x 版本，利用 Vite 的快速构建能力
- 使用 Curve 主题，提供丰富的功能：
  - 自动生成分类、标签、归档页面
  - 支持 RSS 订阅
  - 支持 PWA 离线访问
  - 支持评论系统 (Artalk / Twikoo)
  - 支持 Algolia 搜索
  - 支持音乐播放器
  - 支持图片灯箱放大
- 自动导入和组件注册由 unplugin 处理
- 使用 Pinia 进行状态管理
