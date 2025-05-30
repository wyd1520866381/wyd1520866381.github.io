---
title: 四、建站过程——ButterFly主题配置
date: 2025-05-20 10:00:03
tags:
  - 建站
  - Butterfly
  - Hexo
categories:
  - 前端
  - 建站
series: 建站过程
---

## Butterfly 主题配置过程

### 1. 安装 Butterfly 主题

首先，确保你的 Hexo 博客已经搭建完成。然后，在你的 Hexo 博客根目录下打开终端，运行以下命令安装 Butterfly 主题：

```bash
npm install hexo-theme-butterfly
```

安装完成后，修改 Hexo 根目录下的 `_config.yml` 文件，将 `theme` 的值更改为 `butterfly`。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.yml
theme: butterfly
```

### 2. 创建主题配置文件

为了方便管理和升级主题，建议不要直接修改主题文件夹下的配置文件。可以将 `node_modules` 文件夹下 `hexo-theme-butterfly` 中的 `_config.yml` 文件复制到 Hexo 项目根目录下，并重命名为 `_config.butterfly.yml`。

```bash
cp -i node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```

Hexo 会优先读取根目录下的 `_config.butterfly.yml` 文件作为主题配置文件。

### 3. 基础配置

打开根目录下的 `_config.butterfly.yml` 文件，进行基础配置。以下是一些常用的配置项，可以根据个人喜好进行修改：

#### 网站信息

修改网站的标题、副标题等信息。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# Site Information
subtitle:
  enable: true
  effect: true
  source: 3 # 可以选择不同的副标题来源
  sub: # 如果关闭打字效果，这里的内容会显示

# Avatar
avatar:
  img: /images/avatar.jpg # 设置你的头像图片路径
  effect: false

# Social links
social:
  fas fa-envelope: mailto:your_email@example.com || 邮箱
  fab fa-github: https://github.com/your_github || Github || "#hdhfbb"
```

#### 菜单设置

配置网站导航菜单。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# 菜单设置
menu:
  首页: / || fas fa-home
  项目: /projects/ || fas fa-project-diagram
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  关于我: /about/ || fas fa-user
```

#### 页面布局

调整首页和文章页的布局。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# 页面布局
index_layout: 3 # 首页布局，可以尝试不同的数字
tag_img: "/images/bg1.png" # 标签页顶部图片
index_img: "/images/bg2.png" # 文章页顶部图片
```

### 4. 简洁高级风格配置

要让网站显得简洁高级，可以从以下几个方面进行配置：

#### 颜色和字体

在 `_config.butterfly.yml` 中找到 `colors` 和 `font` 部分，调整主色调、背景色和字体。尝试使用白色、灰色和黑色，以及简洁的无衬线字体。你可以尝试以下配置：

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# colors
# Replace the default colors
# Example:
# theme_color: '#000'
# background_color: '#fff'

# font
# Replace the default fonts
# Example:
# global-font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", sans-serif
# code-font-family: Consolas, Monaco, "Andale Mono", "Ubuntu Mono", monospace
```

你可以根据自己的喜好调整具体的颜色值和字体栈。`-apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", sans-serif` 是一个常用的字体栈，可以优先使用系统字体，达到相对简洁的效果。

#### 移除不必要的元素

检查 `_config.butterfly.yml` 中是否有开启一些你认为不必要的元素，例如侧边栏的小工具、动画效果等。关闭它们可以使页面更加简洁。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# sidebar
# ... 找到 sidebar 相关的配置，关闭不需要的小工具

# effects
# ... 找到动画效果相关的配置，关闭不需要的动画
```

#### 调整布局和间距

虽然主题提供了布局选项，但更精细的调整可能需要修改 CSS。如果你对 CSS 比较熟悉，可以在 `source/styles` 目录下创建自定义 CSS 文件（例如 `source/styles/minimal.css`），并在 `_config.butterfly.yml` 中引入。

```yaml:/Users/apple/workspace/WebWork/MyBlog/_config.butterfly.yml
# inject
# Insert the code to head or foot, before or after the element
# e.g.
# head_end:
#   - <link rel="stylesheet" href="/css/my.css">

# css
# Insert the css file to head
# e.g.
# - /css/my.css

css:
  - /styles/minimal.css
```

在 `source/styles/minimal.css` 中，你可以调整文章内容、图片、标题等的间距，增加留白，使页面看起来更通透。

### 5. 生成和部署

完成配置后，保存文件。然后在 Hexo 博客根目录下运行以下命令生成静态文件并部署：

```bash
hexo clean
hexo g -d
```

通过以上步骤，你应该可以成功安装并配置 Hexo Butterfly 主题，并使其呈现出简洁高级的风格。具体的配置还需要根据你的个人需求和审美进行调整。

{% series %}
