<template>
  <svg width="1600" height="360" viewBox="0 0 1000 360" fill="none" xmlns="http://www.w3.org/2000/svg" :key="'svg-' + isNight" >
    <!-- 主路径 - 减少层次感处理 -->
    <path d="M1116 7H846.5C846.5 7 850.459 12.813 852 17C853.541 21.187 854.104 23.946 855 28.5C855.896 33.054 855.5 57 855.5 57V108C855.5 108 855.417 121.43 855.5 130C855.583 138.57 855.5 152 855.5 152C855.5 152 854.876 170.816 854 175C853.124 179.184 852.334 181.193 851 185C849.666 188.807 846.5 194 846.5 194L1116 195V7Z" fill="#4f85b0"/>

    <!-- 简化后的遮罩组 -->
    <g opacity="1" filter="url(#simplified-filter)">
      <path d="M846.562 194.25H761.875C779.131 194.25 793.125 152.277 793.125 100.5C793.125 48.7233 779.131 6.75 761.875 6.75H846.562C846.562 6.75 849.75 11.9251 851.25 15.5C852.75 19.0749 853.519 21.6922 854.375 25.8125C855.231 29.9328 855.625 38.9375 855.625 38.9375V51.125V100.5V151.438C855.625 151.438 855.881 158.83 855.625 163C855.369 167.17 855.15 169.52 854.375 173.625C853.6 177.73 853.075 180.233 851.562 184.25C850.05 188.267 846.562 194.25 846.562 194.25Z" fill="#5288b7" />
    </g>

    <g opacity="1" filter="url(#simplified-filter)">
      <path d="M761.069 6.75V6.77012C761.275 6.75787 761.487 6.75 761.7 6.75C779.056 6.75 793.125 48.7233 793.125 100.5C793.125 152.277 779.056 194.25 761.7 194.25C761.487 194.25 761.275 194.241 761.069 194.229V194.25L687.5 194.229C710 189.229 729.644 149.223 729.644 101.125C729.644 49.3483 708.537 7.375 682.5 7.375V6.75H761.069Z" fill="#5e94bf" />
    </g>

    <!-- 修复第一个渐变：白天到夜晚的过渡 -->
    <g :style="{
         opacity: delayedOpacity1,
         transition: 'opacity 1s ease-in-out'
       }"
       filter="url(#simplified-filter)">
      <path d="M684.375 6.95663C709.25 8.93544 729.494 50.1067 729.638 100.667C729.781 152.536 709.294 194.339 683.919 194.378C682.894 194.379 681.9 194.336 680.944 194.25H559.375C611.152 194.25 653.125 152.277 653.125 100.5C653.125 48.7236 611.152 6.75031 559.375 6.75031H684.375V6.95663Z" fill="url(#MyGradient1)" />
    </g>

    <!-- 右侧对称部分 -->
    <g opacity="0.05" filter="url(#simplified-filter)">
      <path d="M272.926 6.75H357.613C340.354 6.75 326.363 48.7233 326.363 100.5C326.363 152.277 340.354 194.25 357.613 194.25H272.926C272.926 194.25 269.737 189.075 268.238 185.5C266.739 181.925 265.968 179.308 265.113 175.188C264.257 171.067 263.863 162.062 263.863 162.062V149.875V100.5V49.5625C263.863 49.5625 263.609 42.1702 263.863 38C264.117 33.8298 264.336 31.4801 265.113 27.375C265.889 23.2699 266.412 20.7666 267.926 16.75C269.439 12.7334 272.926 6.75 272.926 6.75Z" fill="#FFFFFF" />
    </g>

    <g opacity="0.175" filter="url(#simplified-filter)">
      <path d="M358.306 194.25V194.23C358.097 194.242 357.887 194.25 357.677 194.25C340.321 194.25 326.25 152.277 326.25 100.5C326.25 48.7233 340.321 6.75 357.677 6.75C357.887 6.75 358.097 6.7585 358.306 6.77075V6.75L431.875 6.77075C409.375 11.7708 389.734 51.7773 389.734 99.875C389.734 151.652 410.839 193.625 436.875 193.625V194.25H358.306Z" fill="#FFFFFF" />
    </g>

    <!-- 修复第二个渐变：夜晚模式特殊元素 -->
    <g :style="{
         opacity: delayedOpacity2,
         transition: 'opacity 1s ease-in-out'
       }"
       filter="url(#simplified-filter)">
      <path d="M434.637 194.171C409.764 192.192 389.517 151.021 389.376 100.461C389.231 48.5916 409.722 6.78813 435.094 6.75006C436.12 6.74851 437.111 6.79169 438.066 6.877H559.637C507.861 6.877 465.887 48.8504 465.887 100.627C465.887 152.404 507.861 194.377 559.637 194.377H434.637V194.171Z" fill="#FFFFFF" />
    </g>

    <defs>
  <!-- 调整滤镜范围，确保覆盖整个遮罩区域 -->
  <filter
    id="simplified-filter"
    x="0"
    y="0"
    width="1200"
    height="360"
    filterUnits="userSpaceOnUse"
    color-interpolation-filters="sRGB"
  >
    <feFlood flood-opacity="0" result="BackgroundImageFix"/>
    <feColorMatrix
      in="SourceAlpha"
      type="matrix"
      values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"
      result="hardAlpha"
    />
    <feOffset dx="1" dy="1"/> <!-- 调整偏移量 -->
    <feGaussianBlur stdDeviation="2"/> <!-- 减少模糊强度 -->
    <feComposite in2="hardAlpha" operator="over"/> <!-- 改为 overlay 避免裁剪 -->
    <feColorMatrix
      type="matrix"
      values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.15 0"/>
    <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
    <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
  </filter>
  <linearGradient id="MyGradient1" x1="0%" y1="0%" x2="100%" y2="0%">
    <stop offset="0%" stop-color="#6DBFF0" />
    <stop offset="100%" stop-color="#6d9fc6" />
  </linearGradient>
</defs>
  </svg>
</template>
<script setup lang="ts">
import { ref, watch } from 'vue'

const props = defineProps({
  isNight: {
    type: Boolean,
    required: true
  }
})

// 用于延迟更新的opacity值
const delayedOpacity1 = ref(1)
const delayedOpacity2 = ref(0)

// 清除定时器的变量
let timer1: ReturnType<typeof setTimeout> | null = null;
let timer2: ReturnType<typeof setTimeout> | null = null;

// 监听isNight变化，延迟更新opacity
watch(() => props.isNight, (newVal) => {
  console.log('isNight changed:', newVal)

  // 清除之前的定时器
  if (timer1) clearTimeout(timer1)
  if (timer2) clearTimeout(timer2)

  // 延迟1500ms后更新opacity
  timer1 = setTimeout(() => {
    delayedOpacity1.value = newVal ? 0.2 : 1
  }, 300)

  timer2 = setTimeout(() => {
    delayedOpacity2.value = newVal ? 0.3 : 0
  }, 50)
})

// 初始化值
delayedOpacity1.value = props.isNight ? 0.2 : 1
delayedOpacity2.value = props.isNight ? 0.3 : 0
</script>