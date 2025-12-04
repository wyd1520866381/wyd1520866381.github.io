---
title: 一、创建一个基础的BabylonJS场景
date: 2025-05-30 12:14:27
tags:
  - BabylonJS
  - WebGL
categories:
  - 图形学
series: BabylonJS
---

## 第一幕：创建你的第一个 Babylon.js 场景

现在是时候创建我们的场景并在屏幕上生成一些东西了。我们将以 Vite 项目为例，从零开始搭建一个基础的 Babylon.js 场景。

### 1. 创建 Vite 项目并安装 Babylon.js

首先，我们需要创建一个 Vite 项目。打开你的终端，执行以下命令：

```bash
npm create vite@latest my-babylonjs-app -- --template vanilla
cd my-babylonjs-app
npm install
```

接下来，安装 Babylon.js 库：

```bash
npm install @babylonjs/core @babylonjs/loaders
```

### 2. 初始化场景要素

在`main.js`（或你选择的入口文件）中，我们需要引入 Babylon.js 并设置场景的核心要素：引擎、场景、相机和灯光。

```javascript
import {
  Engine,
  Scene,
  ArcRotateCamera,
  HemisphericLight,
  Vector3,
  MeshBuilder,
} from "@babylonjs/core";

// 获取 HTML 中的 canvas 元素。在你的 index.html 中添加一个 canvas 标签：<canvas id="renderCanvas"></canvas>
const canvas = document.getElementById("renderCanvas");

// 初始化 Babylon.js 引擎
const engine = new Engine(canvas, true);

// 创建场景
const createScene = () => {
  const scene = new Scene(engine);

  // 创建相机
  // ArcRotateCamera 是一种围绕目标旋转的相机，非常适合观察物体。
  const camera = new ArcRotateCamera(
    "camera",
    -Math.PI / 2,
    Math.PI / 2.5,
    3,
    Vector3.Zero(),
    scene
  );
  camera.attachControl(canvas, true);

  // 创建光源
  // HemisphericLight 是一种半球光，模拟环境光，没有明确的光源方向。
  const light = new HemisphericLight("light", new Vector3(0, 1, 0), scene);
  light.intensity = 0.7;

  // 创建一个球体
  const sphere = MeshBuilder.CreateSphere("sphere", { diameter: 1 }, scene);

  // 移动球体使其在地面上方
  sphere.position.y = 1;

  // 创建一个地面
  const ground = MeshBuilder.CreateGround(
    "ground",
    { width: 6, height: 6 },
    scene
  );

  return scene;
};

const scene = createScene();

// 渲染循环
engine.runRenderLoop(() => {
  scene.render();
});

// 响应窗口大小变化
window.addEventListener("resize", () => {
  engine.resize();
});
```

### 3. 更新 `index.html`

在你的 `index.html` 文件中，确保有一个 `<canvas>` 元素，并且引入你的 JavaScript 文件。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Babylon.js Basic Scene</title>
    <style>
      html,
      body {
        overflow: hidden;
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
      }
      #renderCanvas {
        width: 100%;
        height: 100%;
        touch-action: none;
      }
    </style>
  </head>
  <body>
    <canvas id="renderCanvas"></canvas>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

### 4. 运行你的应用

在终端中运行 Vite 开发服务器：

```bash
npm run dev
```

现在，你可以在浏览器中看到一个球体和一个地面，这就是你创建的第一个 Babylon.js 场景！

### 总结

通过以上步骤，我们成功地创建了一个基础的 Babylon.js 场景，并了解了引擎、场景、相机和灯光这几个核心要素的作用。
