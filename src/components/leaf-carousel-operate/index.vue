<script setup lang="ts">
import { computed } from "vue";

interface LeafCarouselOperateProps {
  allIndex: number;
  currentActiveIndex: number;
  position: string;
  color: string;
  type: string;
}
const props = defineProps<LeafCarouselOperateProps>();
const emits = defineEmits(["change"]);

const carouselOperateWrapperStyle = computed(() => {
  let operateStyle: any = { position: "absolute" };
  switch (props.position) {
    case "center":
      operateStyle.bottom = "5%";
      operateStyle.left = "50%";
      operateStyle.transform = "translateX(-50%)";
      break;
    case "left":
      operateStyle.bottom = "5%";
      operateStyle.left = "5%";
      break;
    case "right":
      operateStyle.bottom = "5%";
      operateStyle.right = "5%";
      break;
  }
  return operateStyle;
});

const carouselOperateItemStyle = computed(() => {
  return {
    background: props.color,
  };
});

function handleClickOperate(index: number) {
  emits("change", index);
}
</script>

<template>
  <div
    class="leaf-carousel-operate-wrapper"
    :style="carouselOperateWrapperStyle"
  >
    <div
      v-for="index in allIndex"
      :key="`leaf-carousel-operate-${index}`"
      :class="[
        `leaf-carousel-operate-item-${props.type}`,
        index === currentActiveIndex ? `active-${props.type}` : '',
      ]"
      :style="carouselOperateItemStyle"
      @click="handleClickOperate(index)"
    ></div>
    <div :class="`empty-${props.type}`" />
  </div>
</template>

<style scope lang="scss">
.leaf-carousel-operate-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.leaf-carousel-operate-item-line {
  position: relative;
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

.active-line {
  width: 30px;
}

.leaf-carousel-operate-item-round {
  position: relative;
  width: 10px;
  height: 10px;
  margin: 0 4px;
  transition: all 0.2s ease;
  border-radius: 100%;
  cursor: pointer;
  opacity: 0.3;

  &:hover {
    width: 20px;
    height: 20px;
    opacity: 0.8;
  }
}

.active-round {
  width: 20px;
  height: 20px;
}

.empty-round {
  height: 20px;
}
</style>
