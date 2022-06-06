<script setup lang="ts">
import { computed, ref, Ref, onMounted, onBeforeUnmount } from "vue";
import { isNumber } from "lodash";

type LeafCarouselPropsType = "common" | "fade";
type LeafCarouselPropsPosition = "top" | "left" | "right" | "bottom";

interface LeafCarouselProps {
  imgList: Array<string>;
  type?: LeafCarouselPropsType;
  duration?: number | string;
  width: number | string;
  height: number | string;
  position?: LeafCarouselPropsPosition;
}
const {
  imgList,
  type = "common",
  duration = 3000,
  width,
  height,
  position = "bottom",
} = defineProps<LeafCarouselProps>();
let timer: Ref<any> = ref(null);
let currentActiveIndex: Ref<number> = ref(0);

onMounted(() => {
  if (!duration || !timer) return;
  timerIntervalChange();
});
onBeforeUnmount(() => {
  timerIntervalClear();
});

const carouselStyle = computed(() => ({
  width: isNumber(width) ? `${width}px` : width,
  height: isNumber(height) ? `${height}px` : height,
}));
const carouselOperateStyle = computed(() => {
  let operateStyle: any = { position: "absolute" };
  switch (position) {
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
  if (currentActiveIndex.value === index) return;
  currentActiveIndex.value = index;
  timerIntervalClear();
  timerIntervalChange();
}
function timerIntervalChange() {
  timer.value = setInterval(() => {
    if (currentActiveIndex.value < Number(imgList.length) - 1) {
      currentActiveIndex.value++;
    } else {
      currentActiveIndex.value = 0;
    }
  }, Number(duration));
}
function timerIntervalClear() {
  clearInterval(timer.value);
  timer.value = null;
}
</script>

<template>
  <div class="leaf-carousel-wrapper" :style="carouselStyle">
    <div class="leaf-carousel-operate-wrapper" :style="carouselOperateStyle">
      <div
        v-for="(item, index) in imgList"
        :key="`leaf-carousel-operate-${index}`"
        :class="[
          'leaf-carousel-operate-item',
          index === currentActiveIndex ? 'active' : '',
        ]"
        @click="handleClickOperate(index)"
      ></div>
    </div>
  </div>
</template>

<style scope lang="scss">
.leaf-carousel-wrapper {
  position: relative;
  background: yellow;
}

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
