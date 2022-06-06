<script setup lang="ts">
import { computed } from "vue";

interface LeafCarouselOperateProps {
  allIndex: number;
  currentActiveIndex: number;
  position: string;
}
const props = defineProps<LeafCarouselOperateProps>();
const emits = defineEmits(["change"]);

const carouselOperateStyle = computed(() => {
  let operateStyle: any = { position: "absolute" };
  switch (props.position) {
    case "top":
      operateStyle.top = "5%";
      operateStyle.left = "50%";
      operateStyle.transform = "translateX(-50%)";
      break;
    case "bottom":
      operateStyle.bottom = "5%";
      operateStyle.left = "50%";
      operateStyle.transform = "translateX(-50%)";
  }
  return operateStyle;
});

function handleClickOperate(index: number) {
  emits("change", index);
}
</script>

<template>
  <div class="leaf-carousel-operate-wrapper" :style="carouselOperateStyle">
    <div
      v-for="index in allIndex"
      :key="`leaf-carousel-operate-${index}`"
      :class="[
        'leaf-carousel-operate-item',
        index === currentActiveIndex ? 'active' : '',
      ]"
      @click="handleClickOperate(index)"
    ></div>
  </div>
</template>

<style scope lang="scss">
.leaf-carousel-operate-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;

  .leaf-carousel-operate-item {
    position: relative;
    background: currentColor;
    width: 20px;
    height: 2px;
    margin: 0 4px;
    transition: all 0.2s ease;
    cursor: pointer;
    opacity: 0.3;

    &:hover {
      width: 30px;
      opacity: 0.8;
    }
  }

  .active {
    width: 30px;
  }
}
</style>
