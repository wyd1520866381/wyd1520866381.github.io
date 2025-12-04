---
title: React 性能优化实战（四）：懒加载与代码分割实践
categories:
  - 技术专题
series:
  - React 性能优化实战
date: 2025-06-15 10:00:00
tags:
  - React
  - 性能优化
---

## 文章简介

介绍 React 应用中的懒加载与代码分割技术，提升首屏加载速度和整体性能体验。

## 主要内容

### 1. 代码分割的原理与意义

代码分割（Code Splitting）是将应用的代码拆分为多个包，按需加载，减少首屏加载体积，提升页面响应速度。React18 支持基于路由、组件等多维度的代码分割。

### 2. React.lazy 与 Suspense 的用法

`React.lazy` 允许你用动态 import 的方式按需加载组件，`Suspense` 用于包裹懒加载组件并自定义加载状态。

```tsx
// src/pages/About.tsx
import React from "react";
export default function About() {
  return <div>About Page</div>;
}

// src/App.tsx
import React, { Suspense, lazy } from "react";
const About = lazy(() => import("./pages/About"));

export default function App() {
  return (
    <div>
      <Suspense fallback={<div>加载中...</div>}>
        <About />
      </Suspense>
    </div>
  );
}
```

### 3. 路由级懒加载与动态 import

配合 React Router v6，可以实现路由级别的懒加载：

```tsx
// src/App.tsx
import React, { Suspense, lazy } from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";

const Home = lazy(() => import("./pages/Home"));
const About = lazy(() => import("./pages/About"));

export default function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>页面加载中...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

### 4. 第三方库的异步加载

对于大型第三方库（如 echarts、lodash），可用动态 import 实现异步加载，减少主包体积：

```tsx
import React, { useEffect, useRef } from "react";

export default function Chart() {
  const ref = useRef<HTMLDivElement>(null);
  useEffect(() => {
    let chart: any;
    import("echarts").then((echarts) => {
      if (ref.current) {
        chart = echarts.init(ref.current);
        chart.setOption({ title: { text: "异步加载 ECharts" } });
      }
    });
    return () => chart && chart.dispose();
  }, []);
  return <div ref={ref} style={{ width: 400, height: 300 }} />;
}
```

### 5. 实战案例与性能对比

以实际项目为例，首页首屏包体积由 1.2MB 降至 400KB，首屏加载时间缩短 60%。可通过 [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) 分析优化效果。

#### 性能分析工具推荐

- [React DevTools Profiler](https://react.dev/learn/profile-performance-with-the-react-devtools)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)

## 个人经验

- 懒加载组件时，建议合理拆分粒度，避免过度分包导致频繁加载。
- Suspense fallback 建议设计友好加载动画，提升用户体验。
- 动态 import 可结合业务场景灵活使用，如按 tab、弹窗、路由等维度拆分。
- 生产环境建议开启 gzip/brotli 压缩，进一步减小包体积。

如需完整项目代码或遇到具体实现问题，欢迎留言交流！

结合项目实践，分享懒加载和代码分割的最佳实践与常见问题。
