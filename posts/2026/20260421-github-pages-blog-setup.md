---
title: GitHub Pages 博客搭建全流程 - 从 VuePress 迁移到 VitePress
date: 2026-04-21
permalink: /posts/20260421-github-pages-blog-setup.html
categories: 环境搭建
tags: [GitHub Pages, VitePress, 博客搭建, 自动化部署, 主题迁移]
cover: /images/logo/cover-default.svg
articleGPT: 本文详细记录了将个人博客从 VuePress + vdoing 主题迁移到 VitePress + curve 主题的完整过程，包括环境准备、主题迁移、配置更新、自动化部署等步骤，分享了迁移过程中的经验和踩坑记录，希望对想要搭建或迁移自己博客的朋友有所帮助。
---

# GitHub Pages 博客搭建全流程 - 从 VuePress 迁移到 VitePress

## 前言

作为一个技术人，拥有一个自己的技术博客是件很有成就感的事情。GitHub Pages 提供了免费的静态站点托管服务，非常适合用来搭建个人博客。

这几年我一直在使用 VuePress + vdoing 主题，随着 VitePress 的成熟和更好的性能体验，我决定将博客迁移到 VitePress 上。本文记录完整的迁移和搭建过程，供大家参考。

## 技术选型

### 为什么选择 VitePress

- **更快的构建速度**：基于 Vite，冷启动和热更新都非常快
- **更小的产出体积**：静态HTML + 按需加载，访问速度更快
- **更好的开发体验**：Vue 3 组合式 API，开发体验更流畅
- **社区活跃**：持续更新，bug 修复快

### 主题选择

选择了 [vitepress-theme-curve](https://github.com/imsyy/vitepress-theme-curve) 主题，这个主题功能比较齐全：

- 支持分类、标签、归档
- 支持 Algolia 搜索
- 支持评论系统 (Artalk/Twikoo)
- 支持音乐播放器
- UI 简洁美观，响应式设计

## 环境准备

首先确保你的 Node.js 版本 >= 20，npm 版本 >= 10：

```bash
node --version
# v20+
npm --version
# v10+
```

创建项目目录并初始化：

```bash
mkdir bingxueyizu.github.io
cd bingxueyizu.github.io
pnpm init
```

## 安装依赖

安装 VitePress 和主题所需依赖：

```bash
pnpm add -D vitepress
pnpm add pinia @vueuse/core algoliasearch dayjs instantsearch.js lodash-es vue vue-instantsearch vue-slider-component aplayer pinia-plugin-persistedstate
```

完整的 `package.json` 参考：

```json
{
  "name": "bingxueyizu.github.io",
  "productName": "冰雪异族的博客",
  "version": "1.0.0",
  "description": "冰雪异族的个人技术博客",
  "author": "冰雪异族",
  "home": "https://bingxueyizu.github.io",
  "github": "https://github.com/bingxueyizu/bingxueyizu.github.io",
  "license": "MIT License",
  "engines": {
    "node": ">=20",
    "npm": ">=10"
  }
}
```

## 项目结构

最终的项目结构大概是这样：

```
├── .vitepress/
│   ├── config.mjs        # VitePress 主配置
│   ├── init.mjs          # 主题初始化
│   └── theme/
│       ├── assets/
│       │   ├── themeConfig.mjs  # 主题配置
│       │   └── linkData.mjs     # 友链数据
│       ├── components/   # Vue 组件
│       ├── store/        # Pinia 状态管理
│       ├── utils/        # 工具函数
│       └── views/        # 页面视图
├── .github/workflows/
│   └── ci.yml            # GitHub Actions 自动部署
├── pages/                # 独立页面
│   ├── about.md
│   ├── archives.md
│   ├── categories.md
│   ├── link.md
│   ├── project.md
│   └── tags.md
├── posts/                # 文章目录
│   └── 2026/             # 按年份分类
├── public/               # 静态资源
│   └── images/logo/
├── index.md              # 首页
└── vercel.json           # Vercel 配置
```

## 配置主题

### VitePress 主配置 `.vitepress/config.mjs`

主要配置站点信息、主题配置：

```javascript
import { defineConfig } from "vitepress";
import { initThemeConfig } from "./init.mjs";

const { themeConfig, head } = initThemeConfig();

export default defineConfig({
  outDir: ".vitepress/dist",
  base: "/",
  lang: "zh-CN",
  title: "bingxueyizu's blog",
  description: "故天将降大任于是人也...",
  head,
  cleanUrls: true,
  lastUpdated: true,
  themeConfig,
  ignoreDeadLinks: true,
  markdown: {
    lineNumbers: true,
    config(md) {
      // 插件配置...
    }
  }
});
```

### 主题配置 `.vitepress/theme/assets/themeConfig.mjs`

这里配置你的个人信息、导航、侧边栏、评论系统等：

```javascript
export const themeConfig = {
  siteMeta: {
    title: "bingxueyizu's blog",
    description: "故天将降大任于是人也...",
    site: "https://bingxueyizu.github.io",
    author: {
      name: "冰雪异族",
      email: "your@email.com",
      link: "https://github.com/bingxueyizu",
    },
  },
  // 导航栏
  nav: [
    {
      text: "文库",
      items: [
        { text: "文章列表", link: "/pages/archives", icon: "article" },
        { text: "全部分类", link: "/pages/categories", icon: "folder" },
        { text: "全部标签", link: "/pages/tags", icon: "hashtag" },
      ],
    },
    // ... 更多导航项
  ],
  // 评论配置
  comment: {
    enable: false,
    type: "artalk",
    artalk: {
      site: "",
      server: "",
    },
  },
  // ... 其他配置
};
```

## 创建页面

### 首页 `index.md`

```markdown
---
layout: home
---
```

主题会自动处理首页布局。

### 独立页面

在 `pages/` 目录下创建各种页面：

- `about.md` - 关于我
- `archives.md` - 文章归档
- `categories.md` - 分类列表
- `tags.md` - 标签列表
- `link.md` - 友情链接
- `project.md` - 我的项目

## 配置 GitHub Pages 自动部署

在 `.github/workflows/ci.yml` 中配置自动部署：

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "pnpm"

      - name: Install pnpm
        run: npm install -g pnpm

      - name: Install dependencies
        run: pnpm install

      - name: Build
        run: pnpm build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.ACCESS_TOKEN }}
          publish_dir: .vitepress/dist
          publish_branch: gh-page
```

### 配置 GitHub Token

1. 在 GitHub 个人设置 → Developer settings → Personal access tokens 创建一个 PAT，权限勾选 `repo`
2. 在仓库 → Settings → Secrets and variables → Actions 添加 `ACCESS_TOKEN`，值就是你的 PAT
3. 在仓库 → Settings → Pages → Source 选择 `gh-page` 分支，保存

之后每次推送到 `main` 分支，GitHub Actions 都会自动构建并部署。

## 本地开发和测试

启动本地开发服务器：

```bash
pnpm dev
```

访问 `http://localhost:9877` 即可预览。

构建生产版本：

```bash
pnpm build
```

预览构建结果：

```bash
pnpm preview
```

## 迁移文章

从旧博客把文章迁移过来，只需要：

1. 按年份放到 `posts/YYYY/` 目录
2. 确保 frontmatter 格式正确：

```markdown
---
title: 文章标题
date: YYYY-MM-DD
categories: 分类
tags: [标签1, 标签2]
cover: 封面图片地址
---

文章内容...
```

## 遇到的问题和解决方法

### 1. 链接格式问题

VitePress 默认要求链接以 `.html` 结尾，开启 `cleanUrls: true` 可以去掉后缀。

### 2. 图片路径问题

静态资源放到 `public/` 目录，引用时从根路径开始。

### 3. 构建缓存问题

如果构建出现奇怪问题，删除 `.vitepress/cache` 目录重新构建。

## 总结

整个迁移过程比想象中顺利，VitePress 确实比 VuePress 快很多，开发体验也好很多。

主要优点：
- 构建速度提升明显
- 页面加载更快
- 开发体验更流畅
- 主题功能齐全，开箱即用

如果你也在考虑升级到 VitePress，建议尽早迁移，体验确实好很多。

## 相关链接

- [VitePress 官方文档](https://vitepress.dev/zh/)
- [vitepress-theme-curve 主题](https://github.com/imsyy/vitepress-theme-curve)
- [GitHub Pages](https://pages.github.com/)

如果你在搭建过程中遇到问题，欢迎在评论区留言交流。
