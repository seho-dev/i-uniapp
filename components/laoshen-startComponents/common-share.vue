<template>
  <view>
    <view class="moke" @click="close" @touchmove="touchmove" v-if="show"></view>
    <view class="box animated" :class="{bounceOutDown: hide,bounceInUp: show}" v-if="show">
      <!-- 分享从下弹出 -->
      <view class="bottom">
        <view class="title">{{title}}</view>
        <view class="itemList">
          <view class="item" v-for="(item,index) in share" :key="index" @click.stop="handelClick(index)">
            <img :src="item.url">
            <view class="head">{{item.name}}</view>
          </view>
        </view>
        <!-- 取消 -->
        <view class="cancle" @click="close">取消</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      hide: false
    };
  },
  watch:{
    show(){
      this.hide = false;
    }
  },
  props: {
    show: {
      type: Boolean,
      default: () => {
        return false;
      }
    },
    share: {
      type: Array,
      default: () => {
        return [];
      }
    },
    title: {
      type: String,
      default: () => {
        return "分享到";
      }
    }
  },
  methods: {
    handelClick(index) {
      this.$emit("clickIndex", index);
    },
    touchmove() {
      this.hide = true;
      setTimeout(() => {
        this.$emit("shareClose");
      }, 800);
    },
    close() {
      this.hide = true;
      // 向上提交关闭
      setTimeout(() => {
        this.$emit("shareClose");
      }, 800);
    }
  }
};
</script>

<style lang="stylus" scoped>
.moke {
  z-index: 999998;
  position: fixed;
  height 100vh;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: rgba(204, 204, 204, 0.5);
}

.box {
  z-index: 999999;
  position: fixed;
  left: 0;
  bottom: 0;
  min-height: auto;

  .bottom {
    width: 100vw;
    height: 200px;
    background: #fff;

    .title {
      min-width: 100px;
      height: auto;
      margin: 0 auto;
      font-size: calc(0.7 * 20px);
      text-align: center;
      padding-top: 20px;
    }

    .itemList {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-wrap: wrap;
      width: 100%;
      height: 120px;

      .item {
        width: 40px;
        height: 50px;
        padding: 10px;
        font-size: calc(0.5 * 20px);

        img {
          width: 100%;
          height: 80%;
        }

        .head {
          text-align: center;
          margin-top: 5px;
        }
      }

      .item:first-child {
        margin-left: 0;
      }
    }

    .cancle {
      width: 100%;
      height: 100px;
      font-size: calc(0.85 * 20px);
      text-align: center;
      line-height: 50px;
    }
  }
}
</style>
