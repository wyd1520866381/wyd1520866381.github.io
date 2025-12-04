---
title: React 性能优化实战（一）：性能瓶颈分析方法
categories:
  - 技术专题
series:
  - React 性能优化实战
date: 2025-05-21 10:00:00
tags:
  - React
  - 性能优化
---

## 文章简介

介绍在 React 项目中常见的性能瓶颈类型，梳理定位和分析性能问题的常用方法与工具，为后续优化打下基础。

## 主要内容

## 性能瓶颈的常见表现

- 首屏加载慢，白屏时间长
- 页面滚动、动画卡顿，FPS 低
- 交互响应迟钝，点击、输入延迟
- 组件频繁重渲染，CPU 占用高
- 内存泄漏，页面长时间运行后变慢甚至崩溃

## 性能分析的基本思路

1. 明确性能目标与关键指标（如 FCP、TTI、LCP、FPS 等）
2. 复现性能问题，收集相关数据
3. 利用工具定位瓶颈（网络、渲染、JS 执行、内存等）
4. 结合代码结构分析，找出根因
5. 制定并验证优化方案

## 浏览器性能分析工具

### 1. Chrome DevTools

- **Performance 面板**：录制页面性能快照，分析 JS 执行、渲染、重排重绘等耗时
- **Memory 面板**：检测内存泄漏、对象分配
- **Network 面板**：分析资源加载瓶颈

#### 常用操作

- 录制性能快照，查找长任务（Long Task）
- 查看 Main 线程耗时分布
- 分析 Timeline，定位卡顿点

### 2. React Profiler

用于分析组件渲染耗时、识别不必要的重渲染。

#### 使用方法（React 18 + TypeScript 示例）

```tsx
import React, { Profiler } from "react";

function onRenderCallback(
  id: string,
  phase: "mount" | "update",
  actualDuration: number,
  baseDuration: number,
  startTime: number,
  commitTime: number,
  interactions: Set<any>
) {
  console.log(`Profiler[${id}] ${phase} - 渲染耗时: ${actualDuration}ms`);
}

export default function App() {
  return (
    <Profiler id="App" onRender={onRenderCallback}>
      {/* 你的应用组件 */}
    </Profiler>
  );
}
```

### 3. 性能数据采集与分析实践

- 在关键页面集成 Web Vitals 采集代码，定期分析数据波动
- 利用 React Profiler 定位组件渲染瓶颈，优化重渲染逻辑
- 通过 Chrome DevTools 录制性能快照，分析 JS 阻塞、长任务等问题

#### Web Vitals 采集代码示例

```ts
import { getCLS, getFID, getLCP, getFCP, getTTFB } from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getLCP(console.log);
getFCP(console.log);
getTTFB(console.log);
```

## 个人经验

- 建议先用 DevTools 录制性能快照，快速定位瓶颈类型
- 复杂页面优先关注首屏加载和交互性能
- 善用 React.memo、useMemo、useCallback 等减少不必要的重渲染
- 结合业务实际，定期复盘性能数据，持续优化

结合实际项目，分享如何快速定位性能瓶颈并制定优化方案。
