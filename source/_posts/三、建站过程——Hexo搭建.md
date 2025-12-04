---
title: 三、建站过程——Hexo搭建
date: 2025-05-20 10:00:02
tags:
  - Hexo
categories:
  - 建站
series: 建站过程
---

## Hexo 搭建过程

### 1. 安装 Node.js 和 Git

node 使用 nvm 管理、git 使用 homebrew 安装即可（windows 使用安装包即可

### 2. 安装 Hexo

打开终端，运行以下命令全局安装 Hexo CLI：

```bash
npm install -g hexo-cli
```

安装完成后，验证 Hexo 是否安装成功：

```bash
hexo version
```

### 3. 初始化博客

选择一个合适的目录，运行以下命令初始化一个新的 Hexo 博客：

```bash
hexo init <folder>
cd <folder>
npm install
```

`<folder>` 是你想要创建博客的文件夹名称。`npm install` 命令会安装博客所需的依赖。

### 4. 生成和预览博客

初始化完成后，可以生成静态文件并在本地预览博客：

```bash
hexo generate
hexo server
```

运行 `hexo server` 后，通常可以在浏览器中访问 `http://localhost:4000` 查看你的博客。

## 部署到 GitHub Pages

将 Hexo 博客部署到 GitHub Pages 是一个常见的做法，可以免费托管你的静态网站。

### 1. 创建 GitHub 仓库

在 GitHub 上创建一个新的仓库，仓库名称格式必须是 `<your_github_username>.github.io`。例如，如果你的 GitHub 用户名是 `octocat`，仓库名称就应该是 `octocat.github.io`。

### 2. 配置 Hexo 部署

编辑博客根目录下的 `_config.yml` 文件，找到 `deploy` 部分，修改为以下内容：

```yaml
deploy:
  type: git
  repo: https://github.com/<your_github_username>/<your_github_username>.github.io.git
  branch: main # 或者 master，取决于你的 GitHub Pages 设置
```

将 `<your_github_username>` 替换为你的 GitHub 用户名。

### 3. 安装 Git 部署插件

在博客根目录下运行以下命令安装 Hexo Git 部署插件：

```bash
npm install hexo-deployer-git --save
```

### 4. 部署博客

运行以下命令生成静态文件并部署到 GitHub Pages：

```bash
hexo clean
hexo generate
hexo deploy
```

第一次部署可能需要输入 GitHub 用户名和密码或 Personal Access Token。部署成功后，访问 `https://<your_github_username>.github.io` 就可以看到你的博客了。

## 配置 GitHub Actions 自动部署

使用 GitHub Actions 可以实现每次推送到 GitHub 仓库时自动生成和部署博客，非常方便。

### 1. 创建 GitHub Actions 工作流文件

在你的博客仓库根目录下创建 `.github/workflows` 目录，并在该目录下创建一个 `.yml` 文件，例如 `pages.yml`。文件内容可以参考你提供的 `/Users/apple/workspace/WebWork/MyBlog/.github/workflows/pages.yml` 文件。

以下是一个示例 `pages.yml` 文件内容：

```yaml:/Users/apple/workspace/WebWork/MyBlog/.github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - main # 监听 main 分支的 push 事件

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          submodules: recursive
      - name: Use Node.js v20.18.0
        uses: actions/setup-node@v4
        with:
          node-version: "20" # 使用 Node.js 20 版本
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install # 安装项目依赖
      - name: Build
        run: npm run build # 运行构建命令，通常是 hexo generate
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public # 上传生成的静态文件目录
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4 # 部署到 GitHub Pages
```

### 2. 配置 GitHub Pages 源

在你的 GitHub 仓库设置中，找到 "Pages" 部分，将 "Source" 设置为 "Deploy from a branch"，并选择 `gh-pages` 分支（如果使用 Hexo 默认部署）或 `main` 分支（如果使用 GitHub Actions 部署到 `main` 分支的 `/root` 目录）。对于使用上述 GitHub Actions 工作流的情况，通常是选择 `main` 分支并让 Actions 负责部署。

### 3. 推送到 GitHub

将你的本地博客文件推送到 GitHub 仓库。GitHub Actions 会自动检测到 push 事件，并开始运行工作流，完成博客的生成和部署。

至此，你的 Hexo 博客就成功搭建并部署到 GitHub Pages，并且配置了 GitHub Actions 实现自动部署。

{% series %}
