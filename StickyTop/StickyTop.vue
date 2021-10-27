<!--
 * @Author: zlj
 * @Date: 2021-07-23 17:01:13
 * @LastEditTime: 2021-09-07 18:16:44
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\StickyTop\StickyTop.vue
-->
<template>
  <div>
    <!-- A区 -->
    <div class="sticky_header">
      <slot name="StickyTop_header"></slot>
    </div>
    <!-- B区代码 -->
    <div
      class="auto_fixed"
      :class="auto_fixed"
      :style="{
        top: offsetTop + 'px',
      }"
    >
      <slot name="StickyTop_fixed"></slot>
    </div>
    <!-- 补位的div，在B区“跳”出文档流之后出现。 -->
    <div
      class="auto_fixed_fake"
      :style="{ display: auto_fixed.fixed ? 'block' : 'none' }"
    ></div>
    <!-- C 区 -->
    <div class="content">
      <slot name="StickyTop_fake"></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 顶部固定距离
    offsetTop: {
      type: [Number, String],
      default: 0,
    },
    // 吸附问题,fixed
    isSticky: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      auto_fixed: {},
    };
  },
  created() {},
  watch: {},
  mounted() {
    // 判断是否来源客户端
    if (process.client) {
      this.$nextTick(function () {
        window.addEventListener("scroll", this.onSicky);
      });
    }
  },
  computed: {},
  methods: {
    onSicky() {
      // 计算滚动的距离
      let scrolled =
        document.documentElement.scrollTop || document.body.scrollTop;
      // 计算A区的高度
      let header_height = null;
      if (document.getElementsByClassName("sticky_header")[0]) {
        header_height =
          document.getElementsByClassName("sticky_header")[0].offsetHeight;
      }
      // console.log("滚动的距离:" + scrolled, "头部的高度:" + header_height);
      // 当滚动的距离等于A区的高度的时候，即是临界点，马上通过auto_fixed变量，给B区添加一个
      // class，让B区浮起来
      this.auto_fixed = {
        auto_fixed: true,
        fixed: this.isSticky == false ? scrolled >= header_height : null,
        sticky: this.isSticky == true ? scrolled >= header_height : null,
      };
    },
  },
  destroyed() {
    window.removeEventListener("scroll", this.onSicky);
  },
};
</script>

<style lang="less" scoped>
.auto_fixed {
  width: 750px;
  text-align: center;
  z-index: 2;
}
.fixed {
  position: fixed;
  width: 100%;
}
.sticky {
  // position: sticky;
  position: fixed;
  width: 100%;
}
</style>