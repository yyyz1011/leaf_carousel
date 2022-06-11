<script setup lang="ts">
import LeafCarousel from "./components/leaf-carousel/index.vue";
</script>

<template>
  <LeafCarousel
    :img-list="['', '', '', '', '']"
    :width="600"
    :height="300"
    operate_position="left"
  />
  <ul>
    <li>更新日志</li>
    <li>20220609 TODO operate切换样式</li>
    <li>20220608 更新leaf-carousel-operate中position参数</li>
    <li>20220606 初始化项目更新github仓库</li>
  </ul>
</template>

<style>
</style>
