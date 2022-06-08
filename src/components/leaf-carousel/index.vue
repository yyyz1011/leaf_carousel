<script setup lang="ts">
import { computed, ref, Ref, onMounted, onBeforeUnmount } from "vue";
import { isNumber } from "lodash";
import LeafCarouselOperate from "../leaf-carousel-operate/index.vue";

type LeafCarouselPropsType = "common" | "fade";
type LeafCarouselPropsOperatePosition = "center" | "left" | "right";
type LeafCarouselPropsOperateType = 'line' | 'round'

interface LeafCarouselProps {
  imgList: Array<string>;
  type?: LeafCarouselPropsType;
  duration?: number | string;
  width: number | string;
  height: number | string;
  operate_position?: LeafCarouselPropsOperatePosition;
  operate_color?: string;
  operate_type?:LeafCarouselPropsOperateType;
}
const {
  imgList,
  type = "common",
  duration = 3000,
  width,
  height,
  operate_position = "center",
  operate_color='currentColor',
  operate_type='round',
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

const carouselWrapperStyle = computed(() => ({
  width: isNumber(width) ? `${width}px` : width,
  height: isNumber(height) ? `${height}px` : height,
}));

const carouselOperateProps = computed(() => ({
  position: operate_position,
  color: operate_color,
  type:operate_type,
  currentActiveIndex: currentActiveIndex.value,
  allIndex: Number(imgList.length),
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
  <div class="leaf-carousel-wrapper" :style="carouselWrapperStyle">
    <LeafCarouselOperate
      :all-index="carouselOperateProps.allIndex"
      :current-active-index="carouselOperateProps.currentActiveIndex"
      :position="carouselOperateProps.position"
      :color="carouselOperateProps.color"
      :type="carouselOperateProps.type"
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
