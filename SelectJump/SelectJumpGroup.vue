<!--
 * @Author: your name
 * @Date: 2021-08-05 10:04:20
 * @LastEditTime: 2021-08-11 14:56:26
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\SelectJump\SelectJumpGroup.vue
-->

<template>
  <div class="u-cell-box">
    <div class="u-cell-title" v-if="title" :style="[titleStyle]">
      {{ title }}
    </div>
    <div
      class="u-cell-item-box"
      :class="{ 'u-border-bottom u-border-top': border }"
    >
      <slot />
    </div>
  </div>
</template>

<script>
/**
 * cellGroup 单元格父组件Group
 * @description cell单元格一般用于一组列表的情况，比如个人中心页，设置页等。搭配u-cell-item
 *
 * @property {String} title 分组标题
 * @property {Boolean} border 是否显示外边框（默认true）
 * @property {Object} title-style 分组标题的的样式，对象形式，如{'font-size': '24px'} 或 {'fontSize': '24px'}
 * @example <u-cell-group title="设置喜好">
 */
export default {
  name: "SelectJumpGroup",
  props: {
    // 分组标题
    title: {
      type: String,
      default: "",
    },
    // 是否显示分组list上下边框
    border: {
      type: Boolean,
      default: true,
    },
    // 分组标题的样式，对象形式，注意驼峰属性写法
    // 类似 {'font-size': '24px'} 和 {'fontSize': '24px'}
    titleStyle: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      index: 0,
    };
  },
};
</script>

<style lang="less" scoped>
.u-cell-box {
  width: 100%;
}

.u-cell-title {
  padding: 30px 32px 10px 32px;
  font-size: 30px;
  text-align: left;
}

.u-cell-item-box {
  background-color: #ffffff;
  flex-direction: row;
}
</style>

