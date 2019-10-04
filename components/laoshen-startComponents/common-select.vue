<template>
  
  <view
    class="box select"
    :class="{borderShadow:selectlist.borderShow || selectlist.borderShow == undefined,top:top}"
  >
   
    <picker
      class="picker"
      style="padding-left: 10px;padding-right: 10px;"
      @change="selectChange"
      mode="selector"
      :range="selectlist.val"
      :value="showIndex"
    >
      <view class="main">
        <text>{{selectlist.val[showIndex]}}</text>
        <text class="iconfont">&#xe7ee;</text>
      </view>
    </picker>
  </view>
</template>
<script>
export default {
  props: {
    selectlist: {
      type: Object,
      default: () => {
        return null;
      }
    },
    showIndex: {
      type: [Number, String],
      default: () => {
        return 0;
      }
    },
    top: {
      type: Boolean,
      default: () => {
        return false;
      }
    }
  },
  // 下拉框触发事件
  methods: {
    selectChange(v) {
      this.index = v.detail.value; // 改变具体展示的值
      // 向上触发事件，传递选择的键和值
      this.$emit("onChange", {
        name: this.selectlist.name,
        val: this.selectlist.val[this.index],
        index: this.index
      });
    }
  }
};
</script>

<style lang="stylus" scoped>
.top {
  z-index: 10000;
}

.borderShadow {
  box-shadow: 0px 0px 5px #ccc;
}

.select {
  
  width: auto;
  height: 32px;
  border-radius: 8px;
  font-size: calc(0.65 * 20px);

  .main {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-wrap: nowrap;
    height 35px;
    text {
      display: inline-block;
    }
  }
}
</style>
