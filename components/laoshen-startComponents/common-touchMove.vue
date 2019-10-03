<template>
  <view class="box" @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend">
    <slot></slot>
  </view>
</template>

<script>
export default {
  data() {
    return {
      x: 0,
      touch: {
        init: false,
        x: 0,
        y: 0,
        on: false
      }
    };
  },
  props: ["touchXY"], // 最小移动距离触发函数
  methods: {
    touchstart(v) {
      this.touch.init = true;
      const touch = v.touches[0];
      this.touch.x = touch.pageX;
      this.touch.y = touch.pageY;
    },
    touchmove(v) {
      if (!this.touch.init) {
        return;
      }
      const touch = v.touches[0];
      // 记录滑动的距离
      const detailX = touch.pageX - this.touch.x;
      const detailY = touch.pageY - this.touch.y;
      // 不能让它向下滑动
      if (Math.abs(detailY) > Math.abs(detailX)) {
        return;
      }
      // 判断传入的最小移动距离，是否满足滑动条件
      if (Math.abs(detailX) < parseInt(this.touchXY)) {
        return;
      } else {
        this.x = detailX; // 移动中把实时滑动的距离保存，结束的时候要根据这个滑动的距离进行一个正负数判断
        this.touch.on = true;
      }
    },
    touchend(v) {
      this.touch.init = false;
      if (this.touch.on) {
        // 满足滑动条件，判断x是正数还是负数
        if (this.x < 0) {
          // 左滑
          this.$emit("onchange", "left");
        } else {
          this.$emit("onchange", "right");
        }
      }
      this.x = 0;
      this.touch = {
        init: false,
        x: 0,
        y: 0,
        on: false
      };
    }
  }
};
</script>

<style>
</style>
