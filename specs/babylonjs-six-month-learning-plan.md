# Babylon.js 六个月学习计划

> 目标：用 24 周时间，从 Babylon.js 基础知识逐步进阶到能够独立完成中大型 3D 可视化、交互应用、工具链整合与 Vue 项目集成。
>
> 学习原则：**官方文档优先、Playground 先跑通、再迁移到本地工程、最后做综合项目**。

---

## 一、学习资源总览

### 1. 官方核心资源

- 官方文档：<https://doc.babylonjs.com/>
- 官方 Playground：<https://playground.babylonjs.com/>
- 官方首页：<https://www.babylonjs.com/>
- Babylon.js 工作流与项目搭建：官方 Workflow / Vite 教程
- Babylon.js 与 Vue：官方社区扩展文档中的 Vue 教程

### 2. 官方工具体系（学习时必须穿插使用）

Babylon.js 不只是一个渲染引擎，也是一整套平台。学习过程中建议按以下顺序接触：

1. **Playground**：最重要的入门与验证工具，用于快速写示例、调试 API、分享 demo。
2. **Sandbox**：用于拖拽查看模型、检查 glTF / glb 资源。
3. **Viewer**：用于快速嵌入模型展示。
4. **GUI Editor**：学习 2D UI 系统。
5. **Node Material Editor (NME)**：学习可视化材质搭建。
6. **Node Geometry Editor (NGE)**：学习程序化几何。
7. **Node Render Graph Editor (NRGE)**：理解高级渲染流程。
8. **Node Particle Editor (NPE)**：学习粒子特效。
9. **Smart Filters Editor (SFE)**：了解后处理和滤镜工作流。

### 3. 学习方法建议

每个知识点按以下 4 步学习：

1. **看官方教程页面**，理解概念。
2. **打开页面对应 Playground 示例**，直接运行。
3. **改 3~5 个参数**，观察效果变化。
4. **把 Playground 代码迁移到本地 Vite / Vue 项目**。

---

## 二、六个月总体路线

- **第 1 个月：Babylon.js 基础入门与场景搭建**
- **第 2 个月：模型、材质、纹理、动画与交互**
- **第 3 个月：工程化开发、模块化 API、资源管理、Vue 集成**
- **第 4 个月：GUI、粒子、后处理、灯光与高级渲染效果**
- **第 5 个月：物理、性能优化、WebXR、复杂场景组织**
- **第 6 个月：综合实战项目、官方工具联动、架构沉淀与作品集输出**

---

## 三、详细月度学习计划

# 第 1 个月：基础入门与第一批 Playground 实践

## 月目标

- 理解 Babylon.js 的基本运行机制。
- 能独立创建一个最小场景。
- 熟悉 Playground 的使用方式。
- 能看懂并编写最常见的 API 调用。

## 第 1 周：认识 Babylon.js 与 Playground

### 学习内容

- Babylon.js 是什么
- 引擎、场景、相机、灯光、网格的关系
- Playground 的使用方式
- 从官方“第一步”与“第一场景”教程入手

### 推荐官方教程

- The Very First Step
- Getting Started - Chapter 1 - First Scene
- Getting Started - Chapter 1 - Firsts

### 本周重点 API

```ts
const canvas = document.getElementById("renderCanvas") as HTMLCanvasElement;
const engine = new BABYLON.Engine(canvas, true);
const scene = new BABYLON.Scene(engine);
const camera = new BABYLON.ArcRotateCamera("camera", Math.PI / 2, Math.PI / 4, 10, BABYLON.Vector3.Zero(), scene);
camera.attachControl(canvas, true);
const light = new BABYLON.HemisphericLight("light", new BABYLON.Vector3(0, 1, 0), scene);
const box = BABYLON.MeshBuilder.CreateBox("box", {}, scene);
engine.runRenderLoop(() => {
  scene.render();
});
```

### 学习任务

- 在 Playground 中独立搭一个 box + camera + light 场景。
- 练习切换不同相机：`ArcRotateCamera`、`FreeCamera`。
- 练习修改网格尺寸、位置、旋转。

### 本周产出

- 3 个 Playground demo：立方体、球体、地面场景。

---

## 第 2 周：坐标系、变换、相机控制

### 学习内容

- 世界坐标 / 本地坐标
- position / rotation / scaling
- 父子节点
- 常见相机控制方式

### 重点 API

```ts
mesh.position.x = 2;
mesh.rotation.y = Math.PI / 4;
mesh.scaling = new BABYLON.Vector3(2, 1, 1);
mesh.parent = parentMesh;
camera.setTarget(BABYLON.Vector3.Zero());
```

### 学习任务

- 创建多个 mesh，练习层级关系。
- 用父子节点实现“太阳 + 行星”简单公转。
- 尝试让相机跟随目标观察物体。

### 推荐练习方向

- 太阳系简化 demo
- 多物体旋转动画

---

## 第 3 周：材质与纹理基础

### 学习内容

- `StandardMaterial` 基础
- 颜色、漫反射、镜面、高光
- 纹理贴图
- UV 的基本理解

### 重点 API

```ts
const material = new BABYLON.StandardMaterial("mat", scene);
material.diffuseColor = new BABYLON.Color3(1, 0, 0);
material.diffuseTexture = new BABYLON.Texture("/textures/wood.jpg", scene);
box.material = material;
```

### 学习任务

- 给 box、ground 添加不同材质。
- 练习切换颜色、透明度、纹理贴图。
- 做一个“带贴图的房间”基础场景。

### 本周建议工具

- Playground
- Sandbox（查看官方模型或自己下载的 glTF/glb 资源）

---

## 第 4 周：基础动画与渲染循环

### 学习内容

- `runRenderLoop`
- 每帧更新
- 简单关键帧动画
- `registerBeforeRender`

### 重点 API

```ts
scene.registerBeforeRender(() => {
  box.rotation.y += 0.01;
});
```

```ts
const animation = new BABYLON.Animation(
  "boxAnimation",
  "position.y",
  30,
  BABYLON.Animation.ANIMATIONTYPE_FLOAT,
  BABYLON.Animation.ANIMATIONLOOPMODE_CYCLE
);
```

### 学习任务

- 做一个旋转立方体。
- 做一个上下浮动小球。
- 试着组合多个动画效果。

### 第 1 月总结项目

**项目：3D 展示小场景**

要求：
- 至少包含 3 个几何体
- 1 个相机
- 1~2 个灯光
- 2 种以上材质
- 1 个持续动画

---

# 第 2 个月：模型、交互、动画系统与官方示例深挖

## 月目标

- 学会导入外部模型。
- 学会场景交互。
- 理解动画、事件、Picking。
- 能读懂更多 Playground 示例。

## 第 5 周：模型导入

### 学习内容

- glTF / glb 基础
- `SceneLoader` 的用法
- 异步加载思路

### 推荐官方教程

- Getting Started - Chapter 1 - Working with Models
- Setup Your First Web App

### 重点 API

```ts
await BABYLON.SceneLoader.ImportMeshAsync("", "/models/", "scene.glb", scene);
```

或模块化写法：

```ts
import { SceneLoader } from "@babylonjs/core/Loading/sceneLoader";
import "@babylonjs/loaders/glTF";

await SceneLoader.ImportMeshAsync("", "/models/", "scene.glb", scene);
```

### 学习任务

- 导入一个 glb 模型。
- 在 Sandbox 中检查模型层级、材质与动画。
- 在代码中调整模型位置与缩放。

---

## 第 6 周：事件系统与拾取交互

### 学习内容

- 鼠标点击拾取
- hover 效果
- `ActionManager`
- 场景输入事件

### 重点 API

```ts
scene.onPointerObservable.add((pointerInfo) => {
  // 根据事件类型处理点击、移动等
});
```

```ts
const pickResult = scene.pick(scene.pointerX, scene.pointerY);
```

### 学习任务

- 点击 mesh 改变颜色。
- 鼠标移入模型时高亮。
- 做一个 3D 菜单 demo。

---

## 第 7 周：动画系统进阶

### 学习内容

- 模型内置动画
- `beginAnimation`
- `AnimationGroup`
- 缓动函数基本概念

### 重点 API

```ts
scene.beginAnimation(mesh, 0, 60, true);
```

```ts
for (const group of scene.animationGroups) {
  group.start(true);
}
```

### 学习任务

- 播放 glb 自带动画。
- 自己写 mesh 位移 / 旋转 / 缩放动画。
- 做一个“点击按钮播放动画”的案例。

---

## 第 8 周：相机、灯光与场景表达

### 学习内容

- 常见灯光类型：Hemispheric / Directional / Point / Spot
- 阴影基础
- 多相机切换思路
- 场景氛围表达

### 学习任务

- 同一个模型用不同灯光照射对比效果。
- 建立一个带主光源和环境光的小型产品展示页。
- 尝试给方向光增加阴影。

### 第 2 月总结项目

**项目：可交互 3D 模型展示页**

要求：
- 导入一个 glb 模型
- 支持鼠标交互
- 支持至少一段动画
- 支持灯光切换或材质切换

---

# 第 3 个月：工程化、模块化 API、Vite 与 Vue 集成

## 月目标

- 从 Playground 迁移到本地工程。
- 学会 NPM 安装与模块化引用。
- 掌握 Babylon.js 在 Vue 项目中的接入方式。
- 理解 API 调用组织方式。

## 第 9 周：离开 Playground，进入本地工程

### 学习内容

- 官方 Workflow 文档
- 官方 Vite 教程
- Babylon.js 包安装方式
- 从 CDN 写法迁移到 ES Module 写法

### 常见安装方式

```bash
npm install @babylonjs/core @babylonjs/loaders
```

如需 GUI：

```bash
npm install @babylonjs/gui
```

### 模块化 API 调用示例

```ts
import { Engine } from "@babylonjs/core/Engines/engine";
import { Scene } from "@babylonjs/core/scene";
import { ArcRotateCamera } from "@babylonjs/core/Cameras/arcRotateCamera";
import { HemisphericLight } from "@babylonjs/core/Lights/hemisphericLight";
import { Vector3 } from "@babylonjs/core/Maths/math.vector";
import { MeshBuilder } from "@babylonjs/core/Meshes/meshBuilder";

const engine = new Engine(canvas, true);
const scene = new Scene(engine);
const camera = new ArcRotateCamera("camera", Math.PI / 2, Math.PI / 4, 8, Vector3.Zero(), scene);
const light = new HemisphericLight("light", new Vector3(0, 1, 0), scene);
const box = MeshBuilder.CreateBox("box", {}, scene);
```

### 学习任务

- 创建一个 Vite + Babylon.js 本地项目。
- 把第 1 月的 Playground 示例迁移过去。
- 理解按需导入和包结构。

---

## 第 10 周：Babylon.js API 调用方式系统化整理

### 学习内容

Babylon.js 常见 API 调用方式可以分成 5 类：

1. **构造函数创建对象**
   - 如 `new Engine(...)`、`new Scene(...)`、`new ArcRotateCamera(...)`
2. **静态工厂方法创建对象**
   - 如 `MeshBuilder.CreateBox(...)`
3. **实例属性赋值**
   - 如 `mesh.position = ...`、`material.diffuseColor = ...`
4. **实例方法调用**
   - 如 `camera.attachControl(...)`、`scene.render()`
5. **异步 API 调用**
   - 如 `SceneLoader.ImportMeshAsync(...)`、`scene.createDefaultXRExperienceAsync()`

### 建议整理成自己的 API 表

| 类别 | 示例 | 说明 |
|---|---|---|
| 构造函数 | `new Scene(engine)` | 创建核心对象 |
| 工厂方法 | `MeshBuilder.CreateSphere(...)` | 快速创建网格 |
| 属性设置 | `mesh.position.x = 1` | 改状态 |
| 实例方法 | `camera.attachControl(canvas, true)` | 执行行为 |
| 异步调用 | `await SceneLoader.ImportMeshAsync(...)` | 加载资源 / 初始化高级功能 |

### 学习任务

- 将自己已学过的 API 按上表整理为笔记。
- 每种类型至少写 5 个例子。

---

## 第 11 周：Vue 3 中使用 Babylon.js

### 学习内容

- 官方 Vue 教程
- 在 `onMounted` 中初始化 Babylon.js
- 在 `onBeforeUnmount` 中释放资源
- 通过 `ref` 获取 canvas
- Vue 与 Babylon 状态同步

### Vue 组件最小示例

```vue
<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from "vue";
import { Engine } from "@babylonjs/core/Engines/engine";
import { Scene } from "@babylonjs/core/scene";
import { ArcRotateCamera } from "@babylonjs/core/Cameras/arcRotateCamera";
import { HemisphericLight } from "@babylonjs/core/Lights/hemisphericLight";
import { Vector3 } from "@babylonjs/core/Maths/math.vector";
import { MeshBuilder } from "@babylonjs/core/Meshes/meshBuilder";

const canvasRef = ref<HTMLCanvasElement | null>(null);
let engine: Engine | null = null;

onMounted(() => {
  const canvas = canvasRef.value;
  if (!canvas) {
    return;
  }

  engine = new Engine(canvas, true);
  const scene = new Scene(engine);
  const camera = new ArcRotateCamera("camera", Math.PI / 2, Math.PI / 3, 8, Vector3.Zero(), scene);
  camera.attachControl(canvas, true);
  new HemisphericLight("light", new Vector3(0, 1, 0), scene);
  MeshBuilder.CreateBox("box", {}, scene);

  engine.runRenderLoop(() => {
    scene.render();
  });

  window.addEventListener("resize", handleResize);
});

const handleResize = () => {
  engine?.resize();
};

onBeforeUnmount(() => {
  window.removeEventListener("resize", handleResize);
  engine?.dispose();
  engine = null;
});
</script>

<template>
  <canvas ref="canvasRef" style="width: 100%; height: 100vh; display: block"></canvas>
</template>
```

### 学习任务

- 在 Vue 3 + Vite 项目中创建 BabylonCanvas 组件。
- 将场景初始化逻辑封装到 `createScene.ts`。
- 学习 Vue props 控制 Babylon 参数，如背景色、模型路径、自动旋转开关。

---

## 第 12 周：Vue 工程化组织与组件通信

### 学习内容

- 场景管理器封装
- composables 封装（如 `useBabylonScene`）
- Vue 响应式数据驱动 Babylon 对象变化
- 组件通信驱动 3D 场景更新

### 学习任务

- 用按钮控制场景中的模型显示隐藏。
- 用滑块控制模型旋转速度。
- 用下拉框切换模型或材质。

### 第 3 月总结项目

**项目：Vue + Babylon.js 产品展示页**

要求：
- 使用 Vue 3 + Vite
- 拆分至少 3 个组件
- 通过 props 或状态管理控制 3D 场景
- 支持模型加载、相机控制、交互反馈

---

# 第 4 个月：GUI、粒子、材质节点与高级渲染

## 月目标

- 学会制作 Babylon GUI 界面。
- 会使用官方可视化编辑器。
- 掌握粒子、后处理、节点材质等高级功能。

## 第 13 周：Babylon GUI

### 学习内容

- `AdvancedDynamicTexture`
- Button / TextBlock / StackPanel
- 2D UI 与 3D 场景联动

### 重点 API

```ts
import { AdvancedDynamicTexture, Button, StackPanel } from "@babylonjs/gui";

const ui = AdvancedDynamicTexture.CreateFullscreenUI("ui");
const button = Button.CreateSimpleButton("btn", "点击我");
ui.addControl(button);
```

### 学习任务

- 创建一个控制面板。
- 用 GUI 按钮切换模型动画、灯光、材质。
- 对照 Playground 中自带 GUI 示例学习。

### 官方工具

- **GUI Editor**：用于理解 GUI 布局和导出思路。

---

## 第 14 周：粒子系统与视觉效果

### 学习内容

- 基础粒子系统
- 发射器、速度、颜色、生命周期
- 火焰、烟雾、星空等效果

### 学习任务

- 做一个火焰粒子 demo。
- 做一个点击触发粒子的效果。

### 官方工具

- **Node Particle Editor (NPE)**

---

## 第 15 周：Node Material Editor 与材质进阶

### 学习内容

- PBRMaterial 基础
- Node Material 思想
- 节点材质可视化编辑
- 从编辑器导出到项目

### 学习任务

- 先用 `PBRMaterial` 做金属、塑料、玻璃效果对比。
- 再用 **Node Material Editor** 做一个自定义流光或渐变材质。
- 将编辑器产物接入 Vue / Vite 项目。

---

## 第 16 周：后处理、环境光照与渲染表达

### 学习内容

- HDR 环境贴图
- 后处理与滤镜
- Bloom、FXAA、色调映射
- 渲染氛围设计

### 学习任务

- 给产品展示场景增加环境贴图。
- 做一个“白天 / 夜晚”切换效果。
- 给镜头增加 bloom 或抗锯齿设置。

### 参考官方内容

- 灯光与环境相关教程
- Day to Night 教程
- Smart Filters / Render Graph 相关官方工具体验

### 第 4 月总结项目

**项目：3D 展厅 / 作品展示界面**

要求：
- 有 GUI 控制面板
- 有粒子或动态视觉效果
- 使用至少一种高级材质
- 有明显的渲染氛围设计

---

# 第 5 个月：复杂应用能力建设

## 月目标

- 掌握性能优化基础。
- 了解物理与碰撞。
- 接触 WebXR。
- 能组织更复杂的场景和业务逻辑。

## 第 17 周：碰撞、物理与移动控制

### 学习内容

- 碰撞基础
- 重力与移动控制
- 物理引擎接入概念
- 角色 / 相机移动

### 学习任务

- 做第一人称漫游 demo。
- 为地面与障碍物添加碰撞逻辑。
- 尝试接入一个简单物理案例。

---

## 第 18 周：性能优化

### 学习内容

- Draw Call 概念
- 实例化与 thin instances
- 冻结矩阵 / 材质优化
- 纹理与模型资源优化
- 调试与性能观察

### 学习任务

- 对比 1000 个 mesh 与实例化 mesh 的差异。
- 学会使用 Babylon Inspector 观察场景状态。
- 对自己的项目做一次性能优化记录。

---

## 第 19 周：WebXR 入门

### 学习内容

- WebXR 基本概念
- Babylon.js 默认 XR 体验
- 输入、传送、交互

### 重点 API

```ts
const xr = await scene.createDefaultXRExperienceAsync();
```

### 学习任务

- 阅读官方 WebXR 入门页。
- 在支持设备或模拟环境中跑通最小 XR 场景。
- 思考普通 3D 应用与 XR 应用的交互区别。

---

## 第 20 周：复杂场景结构设计

### 学习内容

- 场景模块拆分
- 资源加载器封装
- 交互管理器封装
- UI、3D、业务状态三者协同

### 学习任务

- 封装 `SceneManager`、`AssetManager`、`InteractionManager`。
- 做一个小型“数字孪生看板”或“3D 配置器”雏形。

### 第 5 月总结项目

**项目：复杂交互场景 Demo**

候选方向：
- 3D 配置器
- 数字展厅
- 轻量数字孪生可视化
- XR 体验原型

---

# 第 6 个月：综合实战、官方工具联动与作品沉淀

## 月目标

- 独立完成一个综合项目。
- 将官方工具串联到真实开发流程中。
- 形成自己的 Babylon.js 技术体系与作品集。

## 第 21 周：官方工具联动工作流

### 学习内容

将工具接入真实流程：

- **Playground**：验证 API 和快速试验
- **Sandbox**：检查模型与动画
- **Viewer**：快速嵌入式展示
- **GUI Editor**：产出界面布局参考
- **NME**：产出节点材质
- **NGE**：实验程序化几何
- **NRGE**：理解复杂渲染管线
- **NPE**：产出粒子效果
- **SFE**：理解滤镜与后处理工作流

### 学习任务

- 为自己的综合项目至少使用 3 个官方工具。
- 记录“工具 -> 导出 -> 项目接入”的过程。

---

## 第 22 周：综合项目设计

### 推荐项目选题

1. **Vue + Babylon.js 3D 产品配置器**
   - 切换颜色、材质、配件、灯光
2. **数字展厅 / 博物馆导览**
   - 模型热点、信息弹窗、相机漫游
3. **智慧园区 / 数字孪生看板**
   - 建筑模型、状态标识、告警动画
4. **WebXR 展示原型**
   - 适合展示沉浸式交互能力

### 本周任务

- 写功能列表
- 画模块图
- 明确资源来源
- 拆解开发阶段

---

## 第 23 周：项目实现与优化

### 学习任务

- 完成主要功能开发。
- 加入加载流程、错误处理、资源回收。
- 针对模型体积、渲染性能、交互流畅度进行优化。

---

## 第 24 周：总结、复盘、输出作品集

### 最终产出建议

1. 一份完整项目源码
2. 一份项目说明文档
3. 一份 Babylon.js API 学习笔记
4. 一份“Playground 到 Vue 工程”的迁移经验总结
5. 一份个人作品集页面

### 复盘清单

请确认自己是否已经掌握：

- 能独立搭建 Babylon.js 基础场景
- 能导入模型并完成交互
- 能使用 GUI、粒子、材质、后处理
- 能在 Vue 项目中集成 Babylon.js
- 能理解常见 Babylon.js API 的调用方式
- 能借助 Playground / Sandbox / NME 等工具完成工作流
- 能独立完成一个中等复杂度 3D 项目

---

## 四、官方教程与示例使用建议

### 1. 如何看官方教程

建议优先顺序：

1. **Journey / First Step**
2. **Getting Started Chapter 1~2**
3. **Workflow / Vite / 项目搭建**
4. **Vue 集成**
5. **Features Deep Dive**（材质、动画、GUI、WebXR、性能等）

### 2. 如何使用 Playground demo

每个 demo 建议做 4 件事：

- 先跑通
- 看懂 scene 是怎么创建的
- 修改参数验证理解
- 迁移到本地项目

### 3. 推荐建立自己的 Playground 收藏夹分类

- 基础场景
- 相机与灯光
- 材质与纹理
- 模型加载
- 动画
- GUI
- 粒子
- 后处理
- WebXR
- Vue 迁移案例

---

## 五、Babylon.js API 学习清单

## 1. 基础对象

- `Engine`
- `Scene`
- `Vector3`
- `Color3`
- `Color4`
- `Quaternion`

## 2. 相机

- `ArcRotateCamera`
- `FreeCamera`
- `UniversalCamera`
- `FollowCamera`

## 3. 灯光

- `HemisphericLight`
- `DirectionalLight`
- `PointLight`
- `SpotLight`

## 4. Mesh 与构造

- `MeshBuilder.CreateBox`
- `MeshBuilder.CreateSphere`
- `MeshBuilder.CreateGround`
- `TransformNode`

## 5. 材质与纹理

- `StandardMaterial`
- `PBRMaterial`
- `Texture`
- `CubeTexture`

## 6. 加载与资源

- `SceneLoader.ImportMeshAsync`
- `AssetsManager`

## 7. 动画与交互

- `Animation`
- `AnimationGroup`
- `ActionManager`
- `ExecuteCodeAction`
- `PointerObservable`

## 8. GUI 与效果

- `AdvancedDynamicTexture`
- `Button`
- `TextBlock`
- `ParticleSystem`
- `GlowLayer`
- `HighlightLayer`

## 9. 高级能力

- `createDefaultXRExperienceAsync`
- 实例化 / Thin Instances
- 后处理管线
- Node Material

---

## 六、Vue 项目中的推荐目录结构

```text
src/
  components/
    BabylonCanvas.vue
    SceneToolbar.vue
  composables/
    useBabylonEngine.ts
    useSceneControls.ts
  babylon/
    createScene.ts
    loadAssets.ts
    materials.ts
    interactions.ts
  assets/
    models/
    textures/
    env/
  views/
    ProductShowcase.vue
```

### 推荐拆分原则

- **Vue 负责界面与状态管理**
- **Babylon 模块负责 3D 场景创建与更新**
- **资源加载、交互、材质、动画单独拆文件**

---

## 七、每月检查标准

### 第 1 月结束
- 能独立写最小 Babylon 场景
- 熟悉 Playground
- 理解基础对象关系

### 第 2 月结束
- 能导入模型并做基础交互
- 能看懂大部分入门 Playground 示例

### 第 3 月结束
- 能在 Vue 3 + Vite 中完成 Babylon.js 集成
- 能把 Playground 代码迁移到工程中

### 第 4 月结束
- 能做 GUI、粒子、节点材质、后处理
- 对官方工具有初步使用经验

### 第 5 月结束
- 能处理复杂交互、性能优化、XR 入门
- 具备中型项目组织能力

### 第 6 月结束
- 有完整项目作品
- 有自己的 Babylon.js 知识体系文档
- 能独立规划并实现复杂应用

---

## 八、附加建议

1. **每周至少做 3 个 Playground 改造练习**。
2. **每月至少做 1 个完整小项目**，不要只看不练。
3. **遇到 API 不熟时，先查官方文档，再查 Playground 示例**。
4. **尽早进入 Vue / Vite 工程化环境**，不要长期停留在纯 Playground。
5. **学习官方工具时，不只是会打开，还要尝试导出成果并接入项目**。
6. **建立自己的知识库**：记录常用 API、坑点、示例链接、性能优化经验。

---

## 九、推荐的最终学习成果

六个月结束后，你最好拥有：

- 1 份系统化 Babylon.js 学习笔记
- 1 份 Vue + Babylon.js 工程模板
- 3~5 个可展示的 Playground 示例
- 1~2 个完整综合项目
- 1 份自己整理的 Babylon.js 常用 API 手册

如果你愿意，下一步我还可以继续帮你把这份计划再细化成：

1. **按周拆解的打卡版计划**
2. **适合上班族的每周 6~8 小时版本**
3. **配套的 Vue + Babylon.js 项目脚手架目录说明**
4. **带勾选框的任务清单版 markdown**
