<template>
  <div class="fixed top-4 right-4 z-50">
    <div class="flex items-center">
      <!-- 滑槽容器 -->
      <div
        class="switch-track w-40 h-15 rounded-full overflow-hidden relative cursor-pointer transition-all duration-1500 ease-in-out"
        :class="{ 'is-night': isNight, 'is-animating': isAnimating }"
        @click="toggle"
      >

        <!-- 白天背景层（渐变蓝） -->
        <div class="absolute inset-0 bg-day transition-opacity duration-1500 ease-in-out z-10" :class="{ 'opacity-0': isNight }"></div>
        <!-- 夜晚背景层（渐变黑） -->
        <div class="absolute inset-0 bg-night transition-opacity duration-1500 ease-in-out z-10" :class="{ 'opacity-0': !isNight }"></div>

        <!-- 太阳 -->
        <div
          class="absolute bottom-0 left-1 w-16 h-16 transform transition-transform duration-1500 ease-in-out z-20"
          :class="{ 'translate-x-22': isNight }"
          style="filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.2));"
        >
          <SunIcon class="w-full h-full" />
        </div>

        <!-- 月亮 -->
        <div
          class="absolute top-1 w-16 h-16 transform transition-all duration-1500 ease-in-out z-20"
          :class="{ 'translate-x-7': isNight }"
          style="left: 64.5%; margin-left: -32px; filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.2)); transition-delay: 0.1s;"
        >
          <MoonIcon class="w-full h-full" />
        </div>

        <!-- 遮罩（可选） -->
        <div
          class="absolute bottom-0 top-0.5 right-0 w-120 h-25.5 transform transition-transform duration-1500 ease-in-out translate-x-25.5 z-20"
          :class="{ 'translate-x-47.5': isNight }"
          style="filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.2)); margin-bottom: -1px;"
        >
          <MaskIcon class="w-full h-full" :is-night="isNight"/>
        </div>

        <div
          class="absolute bottom-0 w-40 h-40 transform transition-all duration-1500 ease-in-out z-20 translate-y-6 translate-x-4"
          :class="{'translate-y-21':isNight}"
          style="filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.2));"
        >
          <Background class="w-full h-full" />
        </div>

        <!-- 内阴影覆盖层 - 放在最上层，使用伪元素 -->
        <div class="absolute inset-0 rounded-full pointer-events-none z-30 inner-shadow-layer"></div>
      </div>
    </div>
  </div>
</template>

<script>
import SunIcon from '@/components/Sub_assemblies/Svg_Icons/Sun.vue'
import MoonIcon from '@/components/Sub_assemblies/Svg_Icons/Moon.vue'
import MaskIcon from '@/components/Sub_assemblies/Svg_Icons/Mask.vue'
import Background from '@/components/Sub_assemblies/Svg_Icons/Background.vue'

export default {
  components: { SunIcon, MoonIcon, MaskIcon,Background},
  data() {
    return {
      isNight: false,
      isAnimating: false,
      debounceTimer: null,
      lastToggleTime: 0
    };
  },
  methods: {
    toggle() {
      const now = Date.now();

      // 方法1: 时间间隔限制 - 防止在动画进行时触发
      if (this.isAnimating || (now - this.lastToggleTime) < 300) {
        return;
      }

      // 方法2: 防抖处理 - 清除之前的定时器
      if (this.debounceTimer) {
        clearTimeout(this.debounceTimer);
      }

      // 设置防抖定时器
      this.debounceTimer = setTimeout(() => {
        this.performToggle();
      }, 100); // 100ms 防抖延迟
    },

    performToggle() {
      this.isAnimating = true;
      this.lastToggleTime = Date.now();

      // 切换状态
      this.isNight = !this.isNight;
      document.documentElement.classList.toggle('dark', this.isNight);

      // 动画完成后重置状态
      setTimeout(() => {
        this.isAnimating = false;
      }, 1800); // 与 CSS 动画时间保持一致
    }
  },

  // 清理定时器
  beforeDestroy() {
    if (this.debounceTimer) {
      clearTimeout(this.debounceTimer);
    }
  }
};
</script>

<style>
/* 白天渐变背景 */
.bg-day {
  background: #6DBFF0;
}

/* 夜晚渐变背景 */
.bg-night {
  background: linear-gradient(90deg, #1c1e2e 0%, #000000 100%);
}

/* 滑槽基础样式 - 统一的内阴影 */
.switch-track {
  position: relative;
  /* 统一的内阴影 - 环绕效果 */
  box-shadow:
    inset 0 2px 4px rgba(0, 0, 0, 0.3),
    inset 0 -2px 4px rgba(0, 0, 0, 0),
    inset 2px 0 0 rgba(0, 0, 0, 0.2),
    inset -2px 0 0 rgba(0, 0, 0, 0.2);

  transition: box-shadow 1500ms ease-in-out;
}

/* 动画进行时的样式 */
.switch-track.is-animating {
  pointer-events: none; /* 动画时禁用点击 */
  cursor: not-allowed;
}

/* 改进的内阴影覆盖层 - 环绕效果 */
.inner-shadow-layer::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  z-index: 30;

  /* 环绕式阴影 */
  box-shadow:
    inset 0 4px 3px rgba(0, 0, 0, 1),
    inset 0 3px 1px rgba(0, 0, 0, 0.1),
    inset 2px 0 1px rgba(0, 0, 0, 0.3),
    inset -3px 0 1px rgba(0, 0, 0, 0.3);
}
</style>