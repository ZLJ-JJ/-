<!--
 * @Author: zlj
 * @Date: 2021-07-27 16:20:31
 * @LastEditTime: 2021-08-03 15:44:02
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\sd\zhezhao.vue
-->
<template>
  <!-- @touchmove.prevent 禁止滚动 -->
  <div>
    <div class="zhezhao" @click="close" v-if="!isCenter">
      <slot></slot>
    </div>

    <div class="zhezhao" @click="close" v-else @touchmove="touchmove">
      <div class="popContainer popup">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 是否显示居中
    isCenter: {
      type: Boolean,
      default: false,
    },
    // 内部是否滚动
    isRoll: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {};
  },
  computed: {},
  created() {
    // 判断是否来源客户端
    if (process.client) {
      this.$nextTick(function () {
        window.addEventListener("body", this.touchRoll);
      });
    }
  },
  mounted() {},
  watch: {},
  methods: {
    close() {
      this.$emit("cliZhezhao");
    },
    touchRoll() {
      let body = window.document.body;
      if (this.isRoll) {
        body.style.overflow = "hidden";
        body.style.height = "100vh";
      }
    },
    touchmove(e) {
      this.touchRoll();
      if (!this.isRoll) {
        e.preventDefault();
      }
    },
  },
  components: {},
};
</script>

<style scoped lang="less">
.zhezhao {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.65);
  -webkit-transition: opacity 0.3s;
  transition: opacity 0.3s;
  z-index: 10000;
}
.popContainer {
  position: fixed;
  left: 50%;
  top: 50%;
  -webkit-transform: translate3d(-50%, -50%, 0);
  transform: translate3d(-50%, -50%, 0);
}
.popup {
  max-height: 100%;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
  -webkit-transition: -webkit-transform 0.3s ease-out;
  transition: -webkit-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
  transition: transform 0.3s ease-out, -webkit-transform 0.3s ease-out;
}
</style>
