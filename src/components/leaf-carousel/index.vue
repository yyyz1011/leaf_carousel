<script setup lang="ts">
import { computed, ref, Ref, onMounted, onBeforeUnmount } from "vue";
import { isNumber } from "lodash";
import LeafCarouselOperate from "../leaf-carousel-operate/index.vue";

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
let currentActiveIndex: Ref<number> = ref(1);

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

function handleClickOperate(index: number) {
  if (currentActiveIndex.value === index) return;
  currentActiveIndex.value = index;
  timerIntervalClear();
  timerIntervalChange();
}
function timerIntervalChange() {
  timer.value = setInterval(() => {
    if (currentActiveIndex.value <= Number(imgList.length) - 1) {
      currentActiveIndex.value++;
    } else {
      currentActiveIndex.value = 1;
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
    <LeafCarouselOperate
      :all-index="Number(imgList.length)"
      :current-active-index="currentActiveIndex"
      :position="position"
      @change="handleClickOperate"
    ></LeafCarouselOperate>
  </div>
</template>

<style scope lang="scss">
.leaf-carousel-wrapper {
  position: relative;
  background: yellow;
}
</style>
