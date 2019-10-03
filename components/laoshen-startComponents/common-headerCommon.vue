<template>
  <!-- 任务栏 -->
  <view class="warpper" :style="{height: navHeight + 'px'}">
    <view class="box">
      <view class="header">
        <commonStatus></commonStatus>
        <view class="main" :class="{normalFlex: next == false,isLoginFlex: next }">
          <view class="back iconfont" v-if="backShow" @click="back">back</view>
          <!-- left -->
          <view class="title" :class="{left: !backShow}">{{headerTitle}}</view>
          <view class="next iconfont" v-if="next" @click="handelNext">next</view>
          <view class="go iconfont" :class="right.icon" :style="right.style" v-if="right != null && right.type != 'date'" @click="goRight">{{right.name}}</view>
        </view>
      </view>
    </view>
  </view>
</template>
<script>
import commonStatus from "@/components/common/common-status-bar";
export default {
  components: {
    commonStatus
  },
  data() {
    return {
      navHeight: "80"
    };
  },
  created() {
    let self = this;
    // 获取任务栏的高度
    uni.getSystemInfo({
      success: (res) => {
        let taskHeight = res.statusBarHeight;
        self.navHeight = taskHeight + 50;
      }
    });
  },
  props: {
    headerTitle: {
      type: String,
      default: () => {
        return "";
      }
    },
    backShow: {
      type: Boolean,
      default: () => {
        return true;
      }
    },
    next: {
      type: Boolean,
      default: () => {
        return false;
      }
    },
    // 右边自定义字和监听
    right: {
      type: Object,
      default: () => {
        return null;
      }
    }
  },
  methods: {
    goRight() {
      this.$emit(this.right.listen);
    },
    handelNext() {
      this.$emit("next");
    },
    back() {
      this.$emit("back");
      // 倒退一步
      uni.navigateBack({
        delta: 1
      });
    }
  }
};
</script>
<style lang="stylus" scoped>
.left {
  margin-left: 12px;
}

// 定义is_login和普通头部不一样的flex布局
.normalFlex {
  display: flex;
  justify-content: space-start;
  align-items: center;
}

.isLoginFlex {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.warpper {
  width: 100vw;
}

.box {
  z-index: 9998;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  background-image: linear-gradient(to right, #e86504, #d61b0d);
  color: white;

  // 头部
  .header {
    z-index: 9999;
    width: 100%;
    height: auto;

    .main {
      display flex;
      justify-content flex-start;
      align-items center;
      width: 100%;
      position: relative;
      height: 50px;

      &.normal {
        background: #fff;
      }

     

      .back {
        height: 50px;
        line-height: 50px;
        font-size: calc(0.9 * 20px);
        padding-left: 10px;
        padding-right: 12px;
      }

      .next {
        font-size: calc(1.1 * 20px);
        margin-right: 10px;
        transform: rotate(180deg);
      }

      .title {
        font-size: 1rem;
        height: 50px;
        line-height: 50px;
        flex-grow: 1;
      }

      .go {
        // position: absolute;
        // left: 81%;
        // top: 36%;
        font-size: calc(0.8 * 20px);
        color: #fff;
        margin-right 20px;
      }
    }
  }
}
</style>
