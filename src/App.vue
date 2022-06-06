<script setup lang="ts">
import LeafCarousel from "./components/leaf-carousel/index.vue";
</script>

<template>
  <LeafCarousel
    :img-list="['', '', '', '', '']"
    :width="600"
    :height="300"
    position="bottom"
  />
  <ul>
    <li>TODO 日程安排</li>
    <li>
      20220607
      完善leaf-carousel-operate的position属性，包含上下左右，其中上下水平排列，左右垂直排列
    </li>
    <li>
      20220608
      更新leaf-carousel-operate的生效样式，圆点以及线条两种，之后再开发新样式
    </li>
    <li>
      20220609
      初始化leaf-carousel-img组件
    </li>
  </ul>
  <ul>
    <li>更新日志</li>
    <li>20220606 初始化项目更新github仓库</li>
  </ul>
</template>

<style>
</style>
