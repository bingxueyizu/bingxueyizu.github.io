# 冰雪异族的博客

基于 [VitePress](https://vitepress.dev/) 和 [vitepress-theme-curve](https://github.com/imsyy/vitepress-theme-curve) 主题构建的个人技术博客。

## 博客地址

https://bingxueyizu.github.io/

## 技术栈

- [VitePress 1.x](https://vitepress.dev/) - 静态站点生成器
- [vitepress-theme-curve](https://github.com/imsyy/vitepress-theme-curve) - 简洁美观的博客主题
- GitHub Actions - 自动构建部署

## 本地开发

```bash
# 安装依赖
pnpm install

# 启动本地开发服务器
pnpm dev

# 构建生产版本
pnpm build

# 预览构建结果
pnpm preview
```

## 快速创建文章

使用 Claude Code 自定义命令快速创建新文章：

```
/gh-blog
```

命令会自动：
1. 询问文章标题和内容概要
2. 自动推荐分类和标签
3. 生成 frontmatter
4. 创建文章文件
5. 自动提交到 git
6. 只需要执行 `git push` 即可完成发布

## 目录结构

```
├── .vitepress/            # VitePress 配置
│   ├── config.mjs        # 主配置
│   └── theme/            # 主题源码
├── .github/workflows/
│   └── ci.yml             # GitHub Actions 自动部署
├── pages/                 # 独立页面
├── posts/                 # 文章目录（按年份分类）
├── public/                # 静态资源
└── index.md               # 首页
```

## 自动部署

推送到 `main` 分支后，GitHub Actions 自动构建并部署到 `gh-page` 分支，几分钟后即可访问。

## 许可

MIT License
