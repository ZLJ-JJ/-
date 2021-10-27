<template>
  <div>
    <Zhezhao :isCenter="true" :isRoll="false" v-if="value">
      <div class="u-model">
        <div
          v-if="showTitle"
          class="u-model__title u-line-1"
          :style="[titleStyle]"
        >
          {{ title }}
        </div>
        <div class="u-model__content">
          <div :style="[contentStyle]" v-if="$slots.default">
            <slot />
          </div>
          <div v-else class="u-model__content__message" :style="[contentStyle]">
            {{ content }}
          </div>
        </div>
        <div
          class="u-model__footer u-border-top flex"
          v-if="showCancelButton || showConfirmButton"
        >
          <div
            v-if="showCancelButton"
            :hover-stay-time="100"
            hover-class="u-model__btn--hover"
            class="u-model__footer__button"
            :style="[cancelBtnStyle]"
            @click="cancel"
          >
            {{ cancelText }}
          </div>
          <div
            v-if="showConfirmButton || $slots['confirm-button']"
            :hover-stay-time="100"
            :hover-class="asyncClose ? 'none' : 'u-model__btn--hover'"
            class="u-model__footer__button hairline-left"
            :style="[confirmBtnStyle]"
            @click="confirm"
          >
            <slot v-if="$slots['confirm-button']" name="confirm-button"></slot>

            <section v-else>
              <!-- <u-loading
                mode="circle"
                :color="confirmColor"
                v-if="loading"
              ></u-loading> -->
              <div v-if="loading"></div>
              <section v-else>
                {{ confirmText }}
              </section>
            </section>
          </div>
        </div>
      </div>
    </Zhezhao>
  </div>
</template>

<script>
export default {
  name: "u-modal",
  props: {
    // 是否显示Modal
    value: {
      type: Boolean,
      default: false,
    },
    // 层级z-index
    zIndex: {
      type: [Number, String],
      default: "",
    },
    // 标题
    title: {
      type: [String],
      default: "提示",
    },
    // 弹窗宽度，可以是数值(px)，百分比，auto等
    width: {
      type: [Number, String],
      default: 600,
    },
    // 弹窗内容
    content: {
      type: String,
      default: "内容",
    },
    // 是否显示标题
    showTitle: {
      type: Boolean,
      default: true,
    },
    // 是否显示确认按钮
    showConfirmButton: {
      type: Boolean,
      default: true,
    },
    // 是否显示取消按钮
    showCancelButton: {
      type: Boolean,
      default: false,
    },
    // 确认文案
    confirmText: {
      type: String,
      default: "确认",
    },
    // 取消文案
    cancelText: {
      type: String,
      default: "取消",
    },
    // 确认按钮颜色
    confirmColor: {
      type: String,
      default: "#2979ff",
    },
    // 取消文字颜色
    cancelColor: {
      type: String,
      default: "#606266",
    },
    // 圆角值
    borderRadius: {
      type: [Number, String],
      default: 16,
    },
    // 标题的样式
    titleStyle: {
      type: Object,
      default() {
        return {};
      },
    },
    // 内容的样式
    contentStyle: {
      type: Object,
      default() {
        return {};
      },
    },
    // 取消按钮的样式
    cancelStyle: {
      type: Object,
      default() {
        return {};
      },
    },
    // 确定按钮的样式
    confirmStyle: {
      type: Object,
      default() {
        return {};
      },
    },
    // 是否开启缩放效果
    zoom: {
      type: Boolean,
      default: true,
    },
    // 是否异步关闭，只对确定按钮有效
    asyncClose: {
      type: Boolean,
      default: false,
    },
    // 是否允许点击遮罩关闭modal
    maskCloseAble: {
      type: Boolean,
      default: false,
    },
    // 给一个负的margin-top，往上偏移，避免和键盘重合的情况
    negativeTop: {
      type: [String, Number],
      default: 0,
    },
  },
  data() {
    return {
      loading: false, // 确认按钮是否正在加载中
    };
  },
  computed: {
    cancelBtnStyle() {
      return Object.assign(
        {
          color: this.cancelColor,
        },
        this.cancelStyle
      );
    },
    confirmBtnStyle() {
      return Object.assign(
        {
          color: this.confirmColor,
        },
        this.confirmStyle
      );
    },
  },
  watch: {
    // 如果是异步关闭时，外部修改v-model的值为false时，重置内部的loading状态
    // 避免下次打开的时候，状态混乱
    value(n) {
      if (n === true) this.loading = false;
    },
  },
  methods: {
    confirm() {
      // 异步关闭
      if (this.asyncClose) {
        this.loading = true;
      } else {
        this.$emit("input", false);
      }
      this.$emit("confirm");
    },
    cancel() {
      this.$emit("cancel");
      this.$emit("input", false);
      // 目前popup弹窗关闭有一个延时操作，此处做一个延时
      // 避免确认按钮文字变成了"确定"字样，modal还没消失，造成视觉不好的效果
      setTimeout(() => {
        this.loading = false;
      }, 300);
    },
    // 点击遮罩关闭modal，设置v-model的值为false，否则无法第二次弹起modal
    popupClose() {
      this.$emit("input", false);
    },
    // 清除加载中的状态
    clearLoading() {
      this.loading = false;
    },
  },
};
</script>

<style lang="less" scoped>
.u-model {
  height: auto;
  overflow: hidden;
  font-size: 32px;
  background-color: #fff;
  border-radius: 20px;
  width: 600px;

  &__btn--hover {
    background-color: rgb(230, 230, 230);
  }

  &__title {
    padding-top: 48px;
    font-weight: 500;
    text-align: center;
  }

  &__content {
    &__message {
      padding: 48px;
      font-size: 36px;
      text-align: center;
      border-bottom: 1px solid rgb(230, 230, 230);
    }
  }

  &__footer {
    &__button {
      flex: 1;
      height: 100px;
      line-height: 100px;
      font-size: 32px;
      box-sizing: border-box;
      cursor: pointer;
      text-align: center;
      border-radius: 4px;
    }
  }
}
</style>
