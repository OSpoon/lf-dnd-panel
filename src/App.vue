<script setup lang="ts">
import { onMounted, ref } from "vue";
import LogicFlow from "@logicflow/core";
import { DndPanel } from "./extension/dnd-panel";
import "@logicflow/core/dist/style/index.css";
import "@logicflow/extension/lib/style/index.css";

import { icons } from "./icons";

const container = ref();

onMounted(() => {
  const lf = new LogicFlow({
    container: container.value,
    grid: true,
    plugins: [DndPanel],
  });

  lf.extension.dndPanel.setPatternItems([
    {
      group: "GruopA",
      items: [
        {
          label: "选区",
          icon: icons.select,
        },
      ],
    },
    {
      group: "GruopB",
      items: [
        {
          type: "circle",
          text: "开始",
          label: "开始节点",
          icon: icons.start,
        },
        {
          type: "circle",
          text: "结束",
          label: "结束节点",
          icon: icons.end,
          group: "GruopB",
        },
      ],
    },
    {
      group: "GruopC",
      items: [
        { type: "rect", label: "用户任务", icon: icons.task },
        { type: "rect", label: "系统任务", icon: icons.task, group: "GruopC" },
      ],
    },
    {
      group: "GruopD",
      items: [{ type: "diamond", label: "条件判断", icon: icons.condition }],
    },
  ]);

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
