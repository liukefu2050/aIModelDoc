好，这一块属于 **PlayCanvas 进阶 + 拉开差距的核心能力**。
我按你能**真正用得上、写得出来、能解释清楚**的深度来讲，不走学术路线。

下面内容你可以直接当成 **第 9–12 周进阶学习大纲 + 实操指南**。

---

# 一、自定义 Shader（PlayCanvas Shader 深入）

## 你需要达到的目标

不是写完整渲染管线，而是：

* 能 **看懂 + 修改 PlayCanvas 内置 Shader**
* 能 **自己写局部 Shader（效果增强）**
* 能做：

    * 边缘高亮
    * 呼吸光
    * 扫描线
    * 溶解 / 能量流动

---

## 1?? PlayCanvas Shader 架构速览

PlayCanvas 使用的是 **GLSL + Chunk 机制**

### 核心概念

| 概念               | 说明             |
| ---------------- | -------------- |
| Vertex Shader    | 顶点变换           |
| Fragment Shader  | 像素颜色           |
| Chunk            | 可插拔 Shader 代码块 |
| StandardMaterial | PBR 基础材质       |

? **90% 情况下你只需要改 Fragment Chunk**

---

## 2?? 最常用的两种方式（重点）

### ? 方式一：修改 StandardMaterial 的 Chunk（推荐）

```js
material.chunks.emissivePS = `
    vec3 getEmission() {
        return vec3(1.0, 0.0, 0.0);
    }
`;
```

适合：

* 发光
* 高亮
* 呼吸效果

---

### ? 方式二：完全自定义 Shader（进阶）

```js
const shader = new pc.Shader(app.graphicsDevice, {
    attributes: {
        aPosition: pc.SEMANTIC_POSITION
    },
    vshader: `
        attribute vec3 aPosition;
        uniform mat4 matrix_modelViewProjection;
        void main(void) {
            gl_Position = matrix_modelViewProjection * vec4(aPosition, 1.0);
        }
    `,
    fshader: `
        precision mediump float;
        void main(void) {
            gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
        }
    `
});
```

适合：

* 特效模型
* UI 特效
* 非 PBR 物体

---

## 3?? 必练 Shader 小效果（强烈建议）

### ? 呼吸光（Emission + sin）

```glsl
float glow = abs(sin(uTime));
return vec3(0.2, 0.6, 1.0) * glow;
```

### ? 扫描线（世界坐标）

```glsl
float scan = step(0.95, fract(vPosition.y * 2.0));
```

### ? 溶解效果

* 噪声贴图
* discard 像素

```glsl
if (noise < threshold) discard;
```

---

## Shader 阶段成果预期

你能：

* 自定义视觉效果（不靠美术）
* 理解 Shader 性能成本
* 看懂 PlayCanvas 内部 Shader

---

# 二、PBR 材质深入（真正“像工业级”）

## 你需要达到的目标

* 知道 **PBR 为什么真实**
* 知道 **每个贴图到底干嘛**
* 知道 **什么参数最影响性能**

---

## 1?? PlayCanvas PBR 核心参数

| 参数               | 作用   |
| ---------------- | ---- |
| Albedo / Diffuse | 基础颜色 |
| Metalness        | 金属度  |
| Roughness        | 粗糙度  |
| Normal           | 表面细节 |
| AO               | 环境遮蔽 |
| Emissive         | 自发光  |

? 记一句话：
**金属 = 高反射，粗糙 = 模糊反射**

---

## 2?? 贴图使用的正确姿势（非常关键）

### 常见错误 ?

* Roughness / Metalness 乱填
* AO 叠加过重
* 法线贴图方向错

### 正确原则 ?

* 金属：Metalness = 1
* 非金属：Metalness = 0
* Roughness 控制“新旧程度”
* 法线强度别超过 1

---

## 3?? 环境贴图（真实性 50% 来源）

* Skybox
* Prefiltered Cubemap

PlayCanvas 会自动处理：

* IBL（Image Based Lighting）
* BRDF

? **换 Skybox = 换世界质感**

---

## 4?? PBR + Shader 的组合用法

例子：

* PBR 模型 + 自定义 Emissive Chunk
* 金属边缘发光
* 能量武器

---

## PBR 阶段成果预期

你能：

* 判断“材质假不假”
* 和美术对接不再靠感觉
* 做出 **工业级 3D 展示**

---

# 三、后处理（Post Effect：Bloom / Fog）

## 你需要达到的目标

* 知道后处理 **什么时候该用**
* 能写 / 改 PostEffect
* 不把性能炸掉

---

## 1?? 后处理原理（一句话）

> **整张屏幕再渲染一遍**

所以：

* 效果强 = 成本高
* 移动端慎用

---

## 2?? Bloom（发光效果）

### 原理

1. 提取高亮
2. 模糊
3. 叠加回原画面

### PlayCanvas 使用方式

```js
const bloom = new pc.BloomEffect(app.graphicsDevice);
camera.camera.postEffects.addEffect(bloom);
```

可调参数：

* intensity
* threshold

---

## 3?? Fog（雾效）

### 两种 Fog

| 类型          | 场景 |
| ----------- | -- |
| Linear      | 室内 |
| Exponential | 户外 |

```js
app.scene.fog = pc.FOG_EXP;
app.scene.fogColor = new pc.Color(0.5, 0.6, 0.7);
```

---

## 4?? 自定义 PostEffect（进阶）

```js
var MyEffect = function (graphicsDevice) {
    this.shader = new pc.Shader(graphicsDevice, {
        vshader: pc.PostEffect.quadVertexShader,
        fshader: `
            precision mediump float;
            uniform sampler2D uColorBuffer;
            varying vec2 vUv0;
            void main() {
                vec4 color = texture2D(uColorBuffer, vUv0);
                gl_FragColor = vec4(color.rgb * 1.2, 1.0);
            }
        `
    });
};
```

---

## 必做后处理练习

* Bloom 强弱调节
* 雾效远景消失
* 黑白 / 暖色滤镜

---

## 后处理阶段成果预期

你将：

* 会判断“该不该加特效”
* 会控制性能成本
* 具备 **影视级视觉能力**

---

# 最终整合能力（这一步最重要）

当你把 **Shader + PBR + 后处理** 打通后，你将具备：

> **低模 + 程序化效果 = 高质量画面**

这在 Web3D 里非常值钱。

---

## 如果你愿意，我可以下一步直接：

1?? 带你 **手写一个 PBR + Emissive 呼吸光 Shader**
2?? 给你 **PlayCanvas 官方 Shader Chunk 对照表**
3?? 设计一个 **“工业设备发光扫描 Demo”完整方案**
4?? 帮你整理 **Shader / 后处理性能避坑清单**

你选一个，我直接给你落地代码。
