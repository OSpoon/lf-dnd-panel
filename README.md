# LogicFlow 自定义拖拽面板风格

近期有位小伙伴在使用 `logic-flow` 的时候, 对于如何实现自定义拖拽面板风格没有找到思路, 在简单沟通过后, 我觉得可以提供一个简单的示例来帮助大家快速了解;

## 简单的需求分析

## 准备一个基础项目

首先使用 `npm create` 创建一个基于 `vite` 最新版本构建的 `vue + typescript` 项目;

```shell
npm create vite@latest lf-dnd-panel
```

接着删除默认提供的 `HelloWorld.vue` 组件及其引用, 最终的项目目录结构如下;

```
lf-dnd-panel
├─ public
│  └─ vite.svg
├─ src
│  ├─ assets
│  │  └─ vue.svg
│  ├─ App.vue
│  ├─ main.ts
│  ├─ style.css
│  └─ vite-env.d.ts
├─ README.md
├─ index.html
├─ package-lock.json
├─ package.json
├─ tsconfig.json
├─ tsconfig.node.json
└─ vite.config.ts
```

尝试启动项目, 确保一切正常;

```shell
npm run dev
```

## 添加 `logic-flow` 基础代码

首先安装 `logic-flow` 核心依赖;

```shell
npm install @logicflow/core --save
```

接着在 `App.vue` 文件中, 添加 `logic-flow` 核心代码;

```vue
<script setup lang="ts">
import { onMounted, ref } from "vue";
import LogicFlow from "@logicflow/core";
import "@logicflow/core/dist/style/index.css";

const container = ref();

onMounted(() => {
  const lf = new LogicFlow({
    container: container.value,
    grid: true,
  });
  lf.render();
});
</script>

<template>
  <div class="container" ref="container"></div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100%;
}
</style>
```

同时要将下面的样式覆盖掉 `style.css` 文件内容;

```css
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
}
```

## 使用内置拖拽面板

安装 `@logicflow/extension` 扩展依赖, 先看一下内置拖拽面板如何使用;

```shell
npm install @logicflow/extension --save
```

再次修改 `App.vue` 文件内容, 导入 DndPanel 对象及扩展所需要的样式模块;

```typescript
import { DndPanel } from "@logicflow/extension";
import "@logicflow/extension/lib/style/index.css";
```

在实例化 `LogicFlow` 对象时, 通过选项 `plugins` 配置 `DndPanel` 对象;

```typescript
const lf = new LogicFlow({
   ...
   plugins: [DndPanel],
});
```

在实例化 `LogicFlow` 对象后, 通过实例对象 `lf.extension.dndPanel` 中的 `setPatternItems` 方法设置拖拽面板的内容;

```typescript
// icons 是一组图标对象(Base64字符串)
import { icons } from "./icons";

lf.extension.dndPanel.setPatternItems([
  {
    label: "选区",
    icon: icons.select,
  },
  {
    type: "circle",
    text: "开始",
    label: "开始节点",
    icon: icons.start,
  },
  {
    type: "rect",
    label: "用户任务",
    icon: icons.task,
  },
  {
    type: "rect",
    label: "系统任务",
    icon: icons.task,
  },
  {
    type: "diamond",
    label: "条件判断",
    icon: icons.condition,
  },
  {
    type: "circle",
    text: "结束",
    label: "结束节点",
    icon: icons.end,
  },
]);
```

重新预览效果, 可以看到内置拖拽面板已经生效;

![dnd-panel](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/202402271700369.png?token=ACNIKH5EGHCSCZFG5GOIMJ3F3WSQY)

## 自定义拖拽面板风格

在自定义拖拽面板风格时, 我这里选择使用内置拖拽面板的源码并搭配哈罗团队开源的跨平台组件库 [Quarkd](https://vue-quarkd.hellobike.com/#/) 进行二次开发.

### 获取 dnd-panel 源码

在 `src` 目录下创建 `extension/dnd-panel.ts` 文件, 并复制 [dnd-panel 源码](https://github.com/didi/LogicFlow/blob/master/packages/extension/src/components/dnd-panel/index.ts) 到该文件中;

修改 `App.vue` 中 `import` 语句, 导入 `extension/dnd-panel.ts` 文件;

```typescript
// 修改前
import { DndPanel } from "@logicflow/extension";

// 修改后
import { DndPanel } from "./extension/dnd-panel";
```

### 修改 dnd-panel 风格

安装 `quarkd` 依赖;

```shell
npm i quarkd --save
```

在 `extension/dnd-panel` 导入 `collapse` 组件;

```typescript
// ./extension/dnd-panel.ts
import "quarkd/lib/collapse";
```
