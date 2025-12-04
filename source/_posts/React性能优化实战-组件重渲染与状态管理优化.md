---
title: React 性能优化实战（三）：组件重渲染与状态管理优化
categories:
  - 技术专题
series:
  - React 性能优化实战
date: 2025-06-08 10:00:00
tags:
  - React
  - 性能优化
---

## 文章简介

聚焦 React 组件的重渲染机制与状态管理，梳理常见的性能陷阱与优化技巧，提升应用响应速度。

## 主要内容

## 组件重渲染的触发机制

- 父组件重新渲染会导致所有子组件默认也重新渲染（除非被 React.memo 包裹）
- props 变化、state 变化、context 变化都会触发组件重渲染
- forceUpdate、key 变化等也会导致组件重新挂载

### 代码示例：父组件更新导致子组件重渲染

```tsx
import React, { useState, memo } from "react";

type ChildProps = { value: number };

const Child = memo(function Child({ value }: ChildProps) {
  console.log("Child render");
  return <div>{value}</div>;
});

export default function Parent() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>count+1</button>
      <button onClick={() => setOther(other + 1)}>other+1</button>
      <Child value={count} />
    </div>
  );
}
```

点击 other+1 时，Child 组件不会重新渲染。

## props、state 变化对渲染的影响

- props 变化会触发子组件渲染（可用 React.memo 优化）
- state 变化只影响当前组件及其子树
- context 变化会影响所有消费该 context 的组件

## React.memo、useMemo、useCallback 的使用场景

- **React.memo**：用于包裹函数组件，避免 props 未变时的重复渲染
- **useMemo**：缓存计算结果，避免重复计算
- **useCallback**：缓存函数引用，避免因函数变化导致子组件重复渲染

### 代码示例：useMemo/useCallback 优化

```tsx
import React, { useState, useMemo, useCallback } from "react";

function Expensive({ num }: { num: number }) {
  const result = useMemo(() => {
    // 假设是复杂计算
    let sum = 0;
    for (let i = 0; i < 1000000; i++) sum += i * num;
    return sum;
  }, [num]);
  return <div>{result}</div>;
}

export default function Demo() {
  const [n, setN] = useState(1);
  const handleClick = useCallback(() => setN((v) => v + 1), []);
  return (
    <div>
      <button onClick={handleClick}>n+1</button>
      <Expensive num={n} />
    </div>
  );
}
```

## 状态提升与局部状态管理

- 状态提升：将多个组件共享的 state 提升到最近的公共父组件
- 局部状态：只在需要的组件内维护 state，避免全局状态污染
- 合理拆分 state，减少无关组件的重渲染

## Redux/MobX 等状态管理优化实践

- Redux 推荐使用 selector + useSelector，避免全量订阅
- 使用 immer/Redux Toolkit 简化不可变数据操作
- MobX 可通过 @observer 精细控制响应式渲染
- 状态切片、模块化管理，提升可维护性

### 代码示例：Redux useSelector 优化

```tsx
import { useSelector } from "react-redux";

function MyComponent() {
  const value = useSelector((state: any) => state.some.value);
  // 只在 value 变化时渲染
  return <div>{value}</div>;
}
```

## 个人经验

- 实际项目中，优先用 React.memo 包裹纯展示组件
- 复杂组件拆分局部 state，减少全局依赖
- Redux/MobX 状态管理时，注意 selector 粒度，避免全量订阅
- 善用 useMemo/useCallback 优化性能，但避免滥用
- 定期用 Profiler 工具分析渲染瓶颈，持续优化

结合实际项目，分享避免无效渲染和高效管理状态的方法。
