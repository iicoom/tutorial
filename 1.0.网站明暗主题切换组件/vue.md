```html
<template>
  <el-switch v-model="isDark" inline-prompt :active-icon="Moon" :inactive-icon="Sunny" @click="handleChange($event)" />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Sunny, Moon } from '@element-plus/icons-vue'

const isDark = ref(false)

const handleChange = (e: any) => {
  // @ts-ignore
  let transition = document.startViewTransition(() => {
    document.documentElement.classList.toggle("dark");
  });

  transition.ready.then(() => {
    // 获取选中元素的位置信息
    const x = e.clientX;
    const y = e.clientY;

    // 获取画圆的半径
    const radius = Math.sqrt(
      Math.max(x, window.innerWidth - x) ** 2 +
      Math.max(y, window.innerHeight - y) ** 2
    ); // 勾股定理

    const clipPath = [
      `circle(0 at ${x}px ${y}px)`,
      `circle(${radius}px at ${x}px ${y}px)`,
    ];

    const isDark = document.documentElement.classList.contains("dark");
    document.documentElement.animate(
      {
        clipPath: isDark ? clipPath : clipPath.reverse(),
      }, // 在坐标x,y处从半径为0的圆变为半径为radius的圆
      {
        duration: 500,
        pseudoElement: isDark ? "::view-transition-new(root)" : "::view-transition-old(root)",
      }
    );
  });
}
</script>

<style scoped></style>
```

console.log('=============', fetch)