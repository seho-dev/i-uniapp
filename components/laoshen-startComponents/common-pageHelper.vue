<template>
  <view class="box">
    <!-- 分页器 -->
    <!-- 通过总数/分的条数,来循环有几个button -->
    <view class="buttonList">
      <!-- left -->
      <view
        @click="startPageLeftAndRight(activeIndex, 'left')"
        class="left"
        :class="{leftAndRightNone: activeIndex == 0 || buttonList == 0}"
      >上一页</view>
      <view v-for="(item,index) in buttonList" :key="index">
        <button @click="startPage(index)" :class="{active: activeIndex === index}">{{index + 1}}</button>
      </view>
      <!-- right -->
      <view
        @click="startPageLeftAndRight(activeIndex, 'right')"
        class="right"
        :class="{leftAndRightNone: activeIndex == buttonList - 1 || buttonList == 0}"
      >下一页</view>
    </view>
  </view>
</template>

<script>
export default {
  computed: {
    buttonList() {
      return Math.ceil(~~this.total / ~~this.page_size);
    }
  },
  watch: {
    now_page(v) {
      this.activeIndex = v - 1;
    }
  },
  data() {
    return {
      activeIndex: 0
    };
  },
  methods: {
    startPageLeftAndRight(activeIndex, type) {
      if (type === "left") {
        if (activeIndex === 0 || this.buttonList == 0) {
          return;
        } else {
          this.activeIndex --;
          this.$emit("pageChange", this.now_page -1);
        }
      } else {
        if (activeIndex === this.buttonList - 1 || this.buttonList == 0) {
          return;
        } else {
          this.activeIndex ++;
          this.$emit("pageChange", this.now_page +1);
        }
      }
    },
    startPage(index) {
      this.activeIndex = index;
      this.$emit("pageChange", index + 1);
    }
  },
  props: {
    total: {
      type: [String, Number],
      default: () => {
        return 0;
      }
    },
    now_page: {
      type: [String, Number],
      default: () => {
        return 1;
      }
    },
    page_size: {
      type: [String, Number],
      default: () => {
        return 10;
      }
    }
  }
};
</script>

<style lang="stylus" scoped>
.buttonList {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
  height: auto;
  margin-top: 20px;
  padding-bottom: 20px;

  .left {
    font-size: 15px;
    margin-right: 20px;
  }

  .right {
    font-size: 15px;
    margin-left: 20px;
  }

  .leftAndRightNone {
    color: #ccc;
  }

  view {
    width: 25px;
    height: 20px;
    font-size: 14px;
    padding: 0px 5px;
    margin-top: 5px;

    button {
      width: 100%;

      &.active {
        background: red;
        color: #fff;
      }
    }
  }
}
</style>
