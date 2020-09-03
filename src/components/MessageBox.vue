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
    show: {
      type: Boolean,
      default: false
    },
    close: {
      type: String,
      default: ""
    }
  },
  methods: {
    handleClickClose() {
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
