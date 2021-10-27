<!--
 * @Author: your name
 * @Date: 2021-08-06 18:01:09
 * @LastEditTime: 2021-08-30 16:29:31
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\SwipeCell\SwipeCell.vue
-->
<template>
  <div class="delete" :style="{ height: height + 'px' }">
    <div class="slider">
      <div
        class="content"
        @touchstart="touchStart"
        @touchmove="touchMove"
        @touchend="touchEnd"
        :style="deleteSlider"
      >
        <!--  插槽中放具体项目中需要内容     -->
        <slot></slot>
      </div>
      <!--  左滑之后右侧显示的内容按钮     -->
      <div class="remove flex_center2" ref="remove">
        <div v-for="item in handles" :key="item.id" @click="onClick">
          <p>
            {{ item.title }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
var x, y, X, Y, swipeX, swipeY;
export default {
  props: {
    // 当前组件的id
    sliderId: {},
    // 父组件当前移动的组件id
    curSlideID: {},
    sensitivity: {
      // 灵敏度（1 ~ 10）1 => 最细腻（跟手） 10 => 最灵敏
      type: Number,
      default: 5
    },
    handles: {
      type: [Array, Object],
      default: [{ title: "", id: 0 }]
    },
    height: {
      type: [Number, String],
      default: 245
    }
  },
  created() {
    // console.log(this.$t("SwipeCellCOM.title"), "444");
  },
  data() {
    return {
      startX: 0, //触摸开始位置
      endX: 0, //结束位置
      moveX: 0, //滑动时的位置
      disX: 0, //移动距离
      deleteSlider: "", //滑动时的效果
      wd: 0 // 左滑后显示出来的remove元素的宽度，设定为最大左滑距离
    };
  },
  mounted() {
    this.ready();
    // 左滑后显示出来的remove元素的宽度，设定为最大左滑距离
    this.wd = this.$refs.remove.offsetWidth;
    if (this.handles.length > 3) {
      throw new Error("handles more than 3");
    }
    this.sensitivity =
      this.sensitivity > 10 ? 10 : this.sensitivity < 1 ? 5 : this.sensitivity;
  },
  methods: {
    touchStart(ev) {
      ev = ev || event;
      //获取触摸点的坐标targetTouches[0].pageX,Y
      x = ev.touches[0].pageX;
      y = ev.touches[0].pageY;
      swipeX = true;
      swipeY = true;
      //tounches类数组，等于1时表示此时有只有一只手指在触摸屏幕（在touchend时，此属性为空）
      if (ev.touches.length == 1) {
        // 记录开始位置
        this.startX = ev.touches[0].clientX;
      }
    },
    touchMove(ev) {
      ev = ev || event;
      X = ev.touches[0].pageX;
      Y = ev.touches[0].pageY;
      // 滑动时距离浏览器左侧实时距离
      this.moveX = ev.touches[0].clientX;
      //起始位置减去 实时的滑动的距离，得到手指实时偏移距离
      this.disX = this.startX - this.moveX;
      if (swipeX && Math.abs(X - x) - Math.abs(Y - y) > 0) {
        // 阻止事件冒泡
        ev.stopPropagation();
        if (X - x > 10) {
          //右滑
          ev.preventDefault(); // 取消事件的默认动作
          this.deleteSlider = "transform:translateX(0px)"; // 如果是向右滑动或者不滑动，不改变滑块的位置
        }
        if (x - X > 10) {
          //左滑
          ev.preventDefault();
          this.$emit("onSlide", this.sliderId);
          this.deleteSlider =
            "transform:translateX(-" + this.disX * this.sensitivity + "px)"; // 表示左滑了，此时滑块开始滑动
          // 最大也只能等于删除按钮宽度
          if (this.disX * this.sensitivity >= this.wd) {
            this.deleteSlider = "transform:translateX(-" + this.wd + "px)";
          }
        }
        swipeY = false;
      }
      // 上下滑动
      if (swipeY && Math.abs(X - x) - Math.abs(Y - y) < 0) {
        swipeX = false;
      }

      // if (ev.touches.length == 1) {
      //   // 滑动时距离浏览器左侧实时距离
      //   this.moveX = ev.touches[0].clientX;
      //   //起始位置减去 实时的滑动的距离，得到手指实时偏移距离
      //   this.disX = this.startX - this.moveX;
      //   // 如果是向右滑动或者不滑动，不改变滑块的位置
      //   if (this.disX < 0 || this.disX == 0) {
      //     this.deleteSlider = "transform:translateX(0px)";
      //     // 大于0，表示左滑了，此时滑块开始滑动
      //   } else if (this.disX > 0) {
      //     // 告知父组件当前向左滑动的组件id
      //     this.$emit("onSlide", this.sliderId);
      //     //具体滑动距离取的是 手指偏移距离 * sensitivity。
      //     this.deleteSlider =
      //       "transform:translateX(-" + this.disX * this.sensitivity + "px)";

      //     // 最大也只能等于删除按钮宽度
      //     if (this.disX * this.sensitivity >= this.wd) {
      //       this.deleteSlider = "transform:translateX(-" + this.wd + "px)";
      //     }
      //   }
      // }
    },
    touchEnd(ev) {
      ev = ev || event;
      // touchEnd获取值为changedTouches，因为当touchend时，touches的值会被清空，详情可参考https://www.cnblogs.com/mengff/p/6005516.html
      if (swipeX && ev.changedTouches.length == 1) {
        let endX = ev.changedTouches[0].clientX;
        this.disX = this.startX - endX;
        //如果距离小于删除按钮一半,强行回到起点
        if (this.disX * this.sensitivity < this.wd / 2) {
          this.deleteSlider = "transform:translateX(0px)";
        } else {
          //大于一半 滑动到最大值
          this.deleteSlider = "transform:translateX(-" + this.wd + "px)";
        }
      }
    },
    ready() {
      document.addEventListener("click", e => {
        // this.$el.contains(e.target) === 除了自己意外的其他元素，此处的作用是当用户点击其他组件，隐藏当前左滑的组件
        if (!this.$el.contains(e.target) && this.disX !== 0) {
          this.deleteSlider = "transform:translateX(-" + "0px)";
        }
      });
    },
    onClick() {
      // 将用户点击的操作id返回出去
      this.$emit("click");
    }
  },
  watch: {
    // 监听父组件的curSlideID值，和自己的sliderId比较，用以隐藏左滑的组件
    curSlideID(newVal, oldVal) {
      if (this.curSlideID !== this.sliderId && this.disX !== 0) {
        this.deleteSlider = "transform:translateX(-" + "0px)";
      }
    }
  }
};
</script>
<style scoped lang="less">
.delete {
  height: 490px;
}
.slider {
  width: 100%;
  height: 100%;
  position: relative;
  user-select: none;
}
.content {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: #ffffff;
  z-index: 1;
  transition: 0.3s;
}
.remove {
  position: absolute;
  min-width: 120px;
  // min-width: 60px;
  // max-width: 150px;
  height: 100%;
  background: #62a0f6;
  right: 0;
  top: 0;
  color: #fff;
  text-align: center;
  font-size: 26px;
}
// .remove p {
//   min-width: 50px;
//   max-width: 60px;
//   height: 100%;
//   // float: left;
// }
// .remove > p:first-child {
//   background: #5a6bfc;
// }
// .remove > p:nth-child(2) {
//   background: rgb(58, 235, 82);
// }
// .remove > p:last-child {
//   background: none;
// }
</style>
