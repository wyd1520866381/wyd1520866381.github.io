---
title: 二、建站过程——Rspress尝试
date: 2025-05-20 10:00:01
tags:
  - 建站
  - Rspress
  - RsPack
categories:
  - 记录
series: 建站过程
---

## Rspress 尝试过程

Rspress 是一个基于 Rspack 的静态站点生成器，特别适合用于构建文档网站。在框架选型阶段，我也尝试了 Rspress，以下是尝试过程及一些心得。

### Rspress 搭建过程

搭建 Rspress 的过程相对直接，主要参考了其官方文档 <mcurl name="Rspress 快速开始" url="https://rspress.dev/zh/guide/start/getting-started"></mcurl> 步骤大致如下：

1. **初始化项目:** 可以通过脚手架 `npm create rspress@latest` 或手动创建目录并初始化 npm 项目。我选择了手动方式，创建了项目目录并运行 `npm init -y`。
2. **安装 Rspress:** 使用 npm、pnpm、yarn 或 bun 安装 Rspress 作为开发依赖。我使用了 npm：`npm install rspress -D`
3. **创建文档目录和首页:** 按照约定，创建 `docs` 目录并在其中创建 `index.md` 作为首页：`mkdir docs && echo '# Hello world' > docs/index.md`
4. **配置 `package.json` 脚本:** 在 `package.json` 中添加用于开发、构建和预览的脚本：

   ```json
   {
     "scripts": {
       "dev": "rspress dev",
       "build": "rspress build",
       "preview": "rspress preview"
     }
   }
   ```

5. **创建配置文件 `rspress.config.ts`:** 在项目根目录创建配置文件，指定文档根目录等配置：

   ```typescript
   import { defineConfig } from "rspress/config";

   export default defineConfig({
     root: "docs",
   });
   ```

6. **创建 `tsconfig.json`:** Rspress 使用 TypeScript，需要一个 `tsconfig.json` 文件进行配置。

完成以上步骤后，运行 `npm run dev` 即可启动本地开发服务进行预览。

### 注意事项

在尝试过程中，主要需要注意以下几点：

- **依赖管理:** 确保 Node.js 环境正确，并使用 npm、yarn 或 pnpm 等工具管理依赖。
- **配置文件:** `rspress.config.ts` 是核心配置文件，需要根据项目需求进行调整，例如修改主题、添加插件等。
- **文档结构:** Rspress 对文档目录结构有一定约定，遵循这些约定可以更方便地组织内容。
- **MDX 支持:** Rspress 支持 MDX，可以在 Markdown 中直接使用 React 组件，这为内容创作提供了更大的灵活性。

### Rspress 框架优势分析

Rspress 作为基于 Rspack 的静态站点生成器，具有以下优势：

- **性能优异:** 得益于 Rspack 的底层优化，Rspress 在构建速度和性能方面表现出色。
- **现代技术栈:** 支持 React/Vue 等现代前端框架，可以方便地在文档中嵌入交互式组件。
- **MDX 支持:** 强大的 MDX 支持使得文档内容可以更加丰富和动态。
- **专注于文档:** Rspress 的设计目标是构建高质量的文档网站，提供了许多针对文档的优化功能，如侧边栏导航、全文搜索等。

### 最终选择 Hexo 的原因

尽管 Rspress 在构建文档网站方面表现优秀，并且技术栈现代，但我最终还是选择了 Hexo 来搭建个人博客。主要原因在于：

- **定位差异:** Rspress 更侧重于技术文档、组件库文档等结构化内容的展示，而我的核心需求是搭建一个以时间线为主要结构的个人博客。
- **生态成熟度:** 相比 Rspress，Hexo 在个人博客领域拥有更成熟的生态系统，特别是主题和插件资源非常丰富，可以快速实现各种博客特有的功能和样式需求。
- **Hexo + Butterfly 主题的契合度:** Hexo 配合 Butterfly 主题能够非常方便地实现我想要的博客布局和美学风格，其提供的各种小部件和配置项都非常符合个人博客的定制需求。
- **基础建设成本:** 虽然 Rspress 技术先进，但对于个人博客而言，Hexo 提供了更多开箱即用的博客特性，使用 Rspress 可能需要投入更多精力进行基础建设以达到与 Hexo 类似的功能和用户体验。

总的来说，Rspress 是一个非常优秀的文档站点生成器，如果我的需求是构建技术文档或项目文档，Rspress 会是一个非常好的选择。但对于个人博客而言，Hexo 凭借其成熟的生态和专注于博客的特性，更符合我的需求，能够更高效地搭建和维护我的博客。

{% series %}
