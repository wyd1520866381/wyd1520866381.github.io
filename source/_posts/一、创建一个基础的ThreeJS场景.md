---
title: 一、创建一个基础的ThreeJS场景
date: 2025-05-30 12:03:56
tags:
  - ThreeJS
categories:
  - 前端
  - 图形学
  - ThreeJS
series: ThreeJS
---

## 第一幕：创建你的第一个 Three.js 场景

现在是时候创建我们的场景并在屏幕上生成一些东西了。我们将以 Vite 项目为例，从零开始搭建一个基础的 Three.js 场景。

### 1. 创建 Vite 项目并安装 Three.js

首先，我们需要创建一个 Vite 项目。打开你的终端，执行以下命令：

```bash
npm create vite@latest my-threejs-app -- --template vanilla
cd my-threejs-app
npm install
```

接下来，安装 Three.js 库：

```bash
npm install three
```

### 2. 初始化场景要素

在`main.js`（或你选择的入口文件）中，我们需要引入 Three.js 并设置场景的四个核心要素：场景、对象、相机和渲染器。

```javascript
import * as THREE from "three";

// 场景 (Scene)
// 场景就像一个容器。您将对象、模型、粒子、灯光等放入其中，并在某个时候要求 Three.js 渲染该场景。
const scene = new THREE.Scene();

// 对象 (Object)
// 对象可以是许多事物。我们将从一个简单的红色立方体开始。
// 要创建该红色立方体，我们需要创建名为 Mesh 的对象类型。Mesh 是几何体（形状）和材质（外观）的组合。

// 几何体 (Geometry): BoxGeometry 代表一个立方体，参数为长宽高。
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 材质 (Material): MeshBasicMaterial 是一种不受光照影响的基础材质。
// 在 Three.js 中指定颜色的方法有很多种。你可以使用 JS 十六进制（如 0xff0000），字符串十六进制（如 '#ff0000'），颜色名称（如 'red'），或者 Color 类的实例。
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });

// 网格 (Mesh): 将几何体和材质组合成一个可渲染的对象。
const mesh = new THREE.Mesh(geometry, material);

// 将网格添加到场景中。如果不将对象添加到场景中，则无法看到它。
scene.add(mesh);

// 相机 (Camera)
// 摄像头不可见。它更像是一种理论观点。当我们渲染您的场景时，它将从该摄像机的视角进行渲染。
// 我们将使用 PerspectiveCamera（透视相机），它模拟人眼观察世界的方式，近大远小。

// 视野 (Field of View - FOV): 视野是你的视角有多大，以度数表示。75度是一个常用的值。
// 纵横比 (Aspect Ratio): 通常是画布的宽度除以高度。
// 近平面 (Near Plane) 和 远平面 (Far Plane): 渲染的最近和最远距离，超出此范围的对象将不会被渲染。

// 定义渲染尺寸
const sizes = {
  width: window.innerWidth,
  height: window.innerHeight,
};

const camera = new THREE.PerspectiveCamera(
  75,
  sizes.width / sizes.height,
  0.1,
  100
);

// 移动相机位置，使其能够看到场景中的物体。默认情况下，相机和物体都在原点(0,0,0)。
// position 属性是具有三个相关属性的对象：x、y 和 z。默认情况下，Three.js 将前进/后退轴视为 z。
// 要向后移动摄像机，我们需要为 z 属性提供一个正值。
camera.position.z = 3;

// 将相机添加到场景中。虽然不强制，但建议将相机添加到场景中，以避免潜在问题。
scene.add(camera);

// 渲染器 (Renderer)
// 渲染器的工作是进行渲染。它将从摄像机的角度渲染我们的场景，结果会被绘制到画布中。

// 获取 HTML 中的 canvas 元素。在你的 index.html 中添加一个 canvas 标签：<canvas class="webgl"></canvas>
const canvas = document.querySelector("canvas.webgl");

// 创建 WebGLRenderer 实例，并指定渲染目标 canvas。
const renderer = new THREE.WebGLRenderer({
  canvas: canvas,
});

// 设置渲染器尺寸，使其与定义的 sizes 匹配。
renderer.setSize(sizes.width, sizes.height);

// 设置设备像素比，以在高DPI屏幕上获得更好的显示效果。
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

// 首次渲染
// 调用渲染器的 render 方法，并传入场景和相机作为参数，即可进行首次渲染。
renderer.render(scene, camera);

// 响应窗口大小变化
window.addEventListener("resize", () => {
  // 更新尺寸
  sizes.width = window.innerWidth;
  sizes.height = window.innerHeight;

  // 更新相机纵横比
  camera.aspect = sizes.width / sizes.height;
  camera.updateProjectionMatrix(); // 更新投影矩阵

  // 更新渲染器尺寸
  renderer.setSize(sizes.width, sizes.height);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
});

// 动画循环 (可选，但对于交互式场景是必需的)
// 如果你想让场景动起来，你需要一个动画循环。这里我们只是简单地重新渲染。
const animate = () => {
  // 例如，让立方体旋转
  // mesh.rotation.y += 0.01;

  // 重新渲染场景
  renderer.render(scene, camera);

  // 请求下一帧动画
  window.requestAnimationFrame(animate);
};

animate();
```

### 3. 更新 `index.html`

在你的 `index.html` 文件中，确保有一个 `<canvas>` 元素，并且引入你的 JavaScript 文件。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Three.js Basic Scene</title>
    <style>
      body {
        margin: 0;
        overflow: hidden; /* 隐藏滚动条 */
      }
      .webgl {
        display: block;
      }
    </style>
  </head>
  <body>
    <canvas class="webgl"></canvas>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

### 4. 运行你的应用

在终端中运行 Vite 开发服务器：

```bash
npm run dev
```

现在，你可以在浏览器中看到一个红色的立方体，这就是你创建的第一个 Three.js 场景！

### 总结

通过以上步骤，我们成功地创建了一个基础的 Three.js 场景，并了解了场景、对象、相机和渲染器这四个核心要素的作用。
