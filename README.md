# MyBlog

基于 Hexo 框架搭建的个人技术博客，使用 Butterfly 主题，专注于前端技术、团队管理、WebGL/3D 开发等领域的知识分享。

## 🚀 快速开始

### 环境要求

- Node.js >= 14.0.0
- npm 或 pnpm
- Git

### 安装依赖

```bash
# 使用 pnpm（推荐）
pnpm install

# 或使用 npm
npm install
```

### 本地开发

```bash
# 启动本地服务器
pnpm run server
# 或
npm run server

# 访问 http://localhost:4000
```

## 📝 Hexo 常用命令

### 基础命令

```bash
# 初始化博客
hexo init [folder]

# 安装依赖
npm install

# 生成静态文件
hexo generate
# 简写
hexo g

# 启动本地服务器
hexo server
# 简写
hexo s

# 部署到远程站点
hexo deploy
# 简写
hexo d

# 清除缓存文件和已生成的静态文件
hexo clean
```

### 文章管理

```bash
# 新建文章
hexo new "文章标题"
# 简写
hexo n "文章标题"

# 新建草稿
hexo new draft "草稿标题"

# 发布草稿
hexo publish "草稿标题"

# 新建页面
hexo new page "页面名称"
```

### 组合命令

```bash
# 清除缓存并重新生成
hexo clean && hexo generate

# 生成并启动服务器
hexo g && hexo s

# 生成并部署
hexo g && hexo d
```

## 📁 项目文件结构

```tree
MyBlog/
├── .github/                   # GitHub Actions 配置
│   ├── dependabot.yml         # 依赖更新配置
│   └── workflows/
│       └── pages.yml          # 自动部署配置
├── .gitignore                 # Git 忽略文件配置
├── .vercel/                   # Vercel 部署配置
├── _config.yml                # Hexo 主配置文件
├── _config.butterfly.yml      # Butterfly 主题配置
├── package.json               # 项目依赖和脚本
├── pnpm-lock.yaml             # pnpm 锁定文件
├── scaffolds/                 # 文章模板
│   ├── draft.md               # 草稿模板
│   ├── page.md                # 页面模板
│   └── post.md                # 文章模板
├── source/                    # 源文件目录
│   ├── _drafts/               # 草稿文件夹
│   ├── _posts/                # 已发布文章
│   ├── about/                 # 关于页面
│   ├── archives/              # 归档页面
│   ├── categories/            # 分类页面
│   ├── tags/                  # 标签页面
│   ├── projects/              # 项目展示页面
│   ├── images/                # 图片资源
│   └── styles/                # 自定义样式
└── themes/                    # 主题文件夹
```

### 核心目录说明

#### `source/` 目录

- **`_posts/`**: 已发布的文章，支持 Markdown 格式
- **`_drafts/`**: 草稿文章，不会被生成到网站中
- **`images/`**: 图片资源文件
- **页面目录**: `about/`、`archives/`、`categories/`、`tags/` 等

#### 配置文件

- **`_config.yml`**: Hexo 核心配置，包含站点信息、URL、目录、扩展等
- **`_config.butterfly.yml`**: Butterfly 主题专用配置
- **`scaffolds/`**: 新建文章时使用的模板

## 🎨 主题配置

本博客使用 [Butterfly](https://butterfly.js.org/) 主题，主要特性：

- 响应式设计
- 多种代码高亮主题
- 支持数学公式渲染
- 内置搜索功能
- 丰富的插件支持

### 主题自定义

主题配置文件：`_config.butterfly.yml`

自定义样式：`source/styles/`

## 📚 内容分类

### 技术文章

- **前端开发**: React 性能优化、WebGL/Three.js/Babylon.js
- **团队管理**: 团队组建、项目管理、沟通协作、技术分享、激励机制
- **工具与框架**: Hexo、微信小程序、开发工具

### 项目展示

- **3D/AR/VR 项目**: 大空间 LBE、WebGL 游戏
- **小程序项目**: 各类商业小程序应用
- **管理平台**: 中后台管理系统

## 🚀 部署

### GitHub Pages

项目配置了 GitHub Actions 自动部署，推送到 `main` 分支后自动构建并部署到 GitHub Pages。

### Vercel

支持 Vercel 一键部署，配置文件：`.vercel/project.json`

### 手动部署

```bash
# 生成静态文件
hexo generate

# 部署到配置的远程仓库
hexo deploy
```

## 📝 写作指南

### Front Matter 模板

```yaml
---
title: 文章标题
date: 2025-01-01 10:00:00
categories:
  - 分类1
  - 分类2
series:
  - 系列名称
tags:
  - 标签1
  - 标签2
---
```

### 文章规范

1. 使用有意义的文件名（中文或英文）
2. 合理设置分类和标签
3. 添加适当的内链引用
4. 图片放在 `source/images/` 目录下
5. 代码块指定语言类型

## 🛠️ 开发

### 本地调试

```bash
# 清除缓存
hexo clean

# 生成静态文件
hexo generate

# 启动本地服务器（支持热重载）
hexo server --draft
```

### 性能优化

- 图片压缩和 WebP 格式
- CSS/JS 文件压缩
- CDN 加速
- 懒加载

## 📄 许可证

MIT License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

**技术栈**: Hexo + Butterfly + GitHub Actions + Vercel

**联系方式**: [关于页面](/about/)
