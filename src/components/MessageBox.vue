<template>
  <div class="message-box" v-if="show">
    <div class="message-box-main">
      <div
        v-if="close"
        class="message-box-main-close"
        @click="handleClickClose"
      >
        X
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "MessageBox",
  props: {
    show: {// 控制隐藏和显示
      type: Boolean,
      default: false
    },
    close: { //是否显示关闭按钮
      type: String,
      default: ""
    }
  },
  methods: {
    handleClickClose() {
      // 更改父组件数据的状态
      this.$emit("update:show", false);
    }
  }
};
</script>

<style scoped>
.message-box {
  position: fixed;
  right: 0;
  top: 0;
  height: 100vh;
  width: 50vw;
  box-shadow: -2px 0 5px #ccc;
}
.message-box-main {
  position: relative;
}
.message-box-main-close {
  position: absolute;
  right: 20px;
  top: 20px;
  cursor: pointer;
}
</style>
