<!--
 * @Author: zlj
 * @Date: 2021-08-11 17:26:45
 * @LastEditTime: 2021-08-12 15:41:15
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\Tag\Tag.vue
-->
<template>
  <div
    v-if="show"
    :class="[
      disabled ? 'u-disabled' : '',
      'u-size-' + size,
      'u-shape-' + shape,
      'u-mode-' + mode + '-' + type,
    ]"
    class="u-tag"
    :style="[customStyle]"
    @click="clickTag"
  >
    {{ text }}
    <div class="u-icon-wrap" @click.stop>
      <van-icon
        @click="close"
        size="22"
        v-if="closeable"
        :color="closeIconColor"
        name="close"
        class="u-close-icon"
        :style="[iconStyle]"
      ></van-icon>
    </div>
  </div>
</template>

<script>
/**
 * tag 提示
 * @description 该组件一般用于标记和选择
 * @tutorial https://www.udivui.com/components/tag.html
 * @property {String} type 主题类型（默认primary）
 * @property {String} size 标签大小（默认default）
 * @property {String} shape 标签形状（默认square）
 * @property {String} text 标签的文字内容
 * @property {String} bg-color 自定义标签的背景颜色
 * @property {String} border-color 标签的边框颜色
 * @property {String} close-color 关闭按钮的颜色
 * @property {String Number} index 点击标签时，会通过click事件返回该值
 * @property {String} mode 模式选择，见官网说明（默认light）
 * @property {Boolean} closeable 是否可关闭，设置为true，文字右边会出现一个关闭图标（默认false）
 * @property {Boolean} show 标签显示与否（默认true）
 * @event {Function} click 点击标签触发
 * @event {Function} close closeable为true时，点击标签关闭按钮触发
 * @example <u-tag text="雪月夜" type="success" />
 */
export default {
  name: "u-tag",
  // 是否禁用这个标签，禁用的话，会屏蔽点击事件
  props: {
    // 标签类型info、primary、success、warning、error
    type: {
      type: String,
      default: "primary",
    },
    disabled: {
      type: [Boolean, String],
      default: false,
    },
    // 标签的大小，分为default（默认），mini（较小）
    size: {
      type: String,
      default: "default",
    },
    // tag的形状，circle（两边半圆形）, square（方形，带圆角），circleLeft（左边是半圆），circleRight（右边是半圆）
    shape: {
      type: String,
      default: "square",
    },
    // 标签文字
    text: {
      type: [String, Number],
      default: "",
    },
    // 背景颜色，默认为空字符串，即不处理
    bgColor: {
      type: String,
      default: "",
    },
    // 标签字体颜色，默认为空字符串，即不处理
    color: {
      type: String,
      default: "",
    },
    // 镂空形式标签的边框颜色
    borderColor: {
      type: String,
      default: "",
    },
    // 关闭按钮图标的颜色
    closeColor: {
      type: String,
      default: "",
    },
    // 点击时返回的索引值，用于区分例遍的数组哪个元素被点击了
    index: {
      type: [Number, String],
      default: "",
    },
    // 模式选择，dark|light|plain
    mode: {
      type: String,
      default: "light",
    },
    // 是否可关闭
    closeable: {
      type: Boolean,
      default: false,
    },
    // 是否显示
    show: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {};
  },
  computed: {
    customStyle() {
      let style = {};
      // 文字颜色（如果有此值，会覆盖type值的颜色）
      if (this.color) style.color = this.color;
      // tag的背景颜色（如果有此值，会覆盖type值的颜色）
      if (this.bgColor) style.backgroundColor = this.bgColor;
      // 如果是镂空型tag，没有传递边框颜色（borderColor）的话，使用文字的颜色（color属性）
      if (this.mode == "plain" && this.color && !this.borderColor)
        style.borderColor = this.color;
      else style.borderColor = this.borderColor;
      return style;
    },
    iconStyle() {
      if (!this.closeable) return;
      let style = {};
      if (this.size == "mini") style.fontSize = "20px";
      else style.fontSize = "22px";
      if (this.mode == "plain" || this.mode == "light") style.color = this.type;
      else if (this.mode == "dark") style.color = "#ffffff";
      if (this.closeColor) style.color = this.closeColor;
      return style;
    },
    // 关闭图标的颜色
    closeIconColor() {
      // 如果定义了关闭图标的颜色，就用此值，否则用字体颜色的值
      // 如果上面的二者都没有，如果是dark深色模式，图标就为白色
      // 最后如果上面的三者都不合适，就返回type值给图标获取颜色
      let color = "";
      if (this.closeColor) return this.closeColor;
      else if (this.color) return this.color;
      else if (this.mode == "dark") return "#ffffff";
      else return this.type;
    },
  },
  methods: {
    // 标签被点击
    clickTag() {
      // 如果是disabled状态，不发送点击事件
      if (this.disabled) return;
      this.$emit("click", this.index);
    },
    // 点击标签关闭按钮
    close() {
      this.$emit("close", this.index);
    },
  },
};
</script>

<style lang="less" scoped>
@u-main-color: #303133;
@u-content-color: #606266;
@u-tips-color: #909399;
@u-light-color: #c0c4cc;
@u-border-color: #e4e7ed;
@u-bg-color: #f3f4f6;

@u-type-primary: #2979ff;
@u-type-primary-light: #ecf5ff;
@u-type-primary-disabled: #a0cfff;
@u-type-primary-dark: #2b85e4;

@u-type-warning: #ff9900;
@u-type-warning-disabled: #fcbd71;
@u-type-warning-dark: #f29100;
@u-type-warning-light: #fdf6ec;

@u-type-success: #19be6b;
@u-type-success-disabled: #71d5a1;
@u-type-success-dark: #18b566;
@u-type-success-light: #dbf1e1;

@u-type-error: #fa3534;
@u-type-error-disabled: #fab6b6;
@u-type-error-dark: #dd6161;
@u-type-error-light: #fef0f0;

@u-type-info: #909399;
@u-type-info-disabled: #c8c9cc;
@u-type-info-dark: #82848a;
@u-type-info-light: #f4f4f5;

@u-form-item-height: 70px;
@u-form-item-border-color: #dcdfe6;

.u-tag {
  box-sizing: border-box;
  align-items: center;
  border-radius: 6px;
  /* #ifndef APP-NVUE */
  display: inline-block;
  /* #endif */
  line-height: 1;
}

.u-size-default {
  font-size: 22px;
  padding: 12px 22px;
}

.u-size-mini {
  font-size: 20px;
  padding: 6px 12px;
}

.u-mode-light-primary {
  background-color: @u-type-primary-light;
  color: @u-type-primary;
  border: 1px solid @u-type-primary-disabled;
}

.u-mode-light-success {
  background-color: @u-type-success-light;
  color: @u-type-success;
  border: 1px solid @u-type-success-disabled;
}

.u-mode-light-error {
  background-color: @u-type-error-light;
  color: @u-type-error;
  border: 1px solid @u-type-error-disabled;
}

.u-mode-light-warning {
  background-color: @u-type-warning-light;
  color: @u-type-warning;
  border: 1px solid @u-type-warning-disabled;
}

.u-mode-light-info {
  background-color: @u-type-info-light;
  color: @u-type-info;
  border: 1px solid @u-type-info-disabled;
}

.u-mode-dark-primary {
  background-color: @u-type-primary;
  color: #ffffff;
}

.u-mode-dark-success {
  background-color: @u-type-success;
  color: #ffffff;
}

.u-mode-dark-error {
  background-color: @u-type-error;
  color: #ffffff;
}

.u-mode-dark-warning {
  background-color: @u-type-warning;
  color: #ffffff;
}

.u-mode-dark-info {
  background-color: @u-type-info;
  color: #ffffff;
}

.u-mode-plain-primary {
  background-color: #ffffff;
  color: @u-type-primary;
  border: 1px solid @u-type-primary;
}

.u-mode-plain-success {
  background-color: #ffffff;
  color: @u-type-success;
  border: 1px solid @u-type-success;
}

.u-mode-plain-error {
  background-color: #ffffff;
  color: @u-type-error;
  border: 1px solid @u-type-error;
}

.u-mode-plain-warning {
  background-color: #ffffff;
  color: @u-type-warning;
  border: 1px solid @u-type-warning;
}

.u-mode-plain-info {
  background-color: #ffffff;
  color: @u-type-info;
  border: 1px solid @u-type-info;
}

.u-disabled {
  opacity: 0.55;
}

.u-shape-circle {
  border-radius: 100px;
}

.u-shape-circleRight {
  border-radius: 0 100px 100px 0;
}

.u-shape-circleLeft {
  border-radius: 100px 0 0 100px;
}

.u-close-icon {
  margin-left: 14px;
  font-size: 22px;
  color: @u-type-success;
}

.u-icon-wrap {
  display: inline-flex;
  transform: scale(0.86);
}
</style>