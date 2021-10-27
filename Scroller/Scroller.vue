<!--
 * @Author: your name
 * @Date: 2021-07-26 15:46:27
 * @LastEditTime: 2021-08-28 10:45:49
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\Scroller\Scroll.vue
-->
<template>
  <div class="operationBox">
    <!-- @scroll="scroll" -->
    <div
      class="content"
      ref="content"
      @touchstart="startTouch"
      @touchmove="move"
      @touchend="endTouch"
    >
      <div
        class="down"
        :style="{
          'background-color': downPullObject.backgroundColor,
          color: downPullObject.fontColor,
          'font-size': downPullObject.fontSize + 'px',
          height: +top + 'px',
          'line-height': +top + 'px'
        }"
        ref="down"
      >
        {{ downPullObject.displayText }}
      </div>
      <div class="enclose" :style="'top:' + top + 'px;'" ref="enclose">
        <slot></slot>
        <div v-if="downPullHintText">
          <div class="flex_center2 mt_30">
            _____ {{ downPullObject.hintText }} _____
          </div>
          <div style="height: 100px"></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      originY: 0, //起点
      targetY: 0, //手指滑动位置
      updataOriginY: 0, //滑动到顶部的时候记录一个起点，用户做滑动到底的动画
      top: 0, //下拉元素距离顶部的距离
      historyTop: 0, //top的缓存
      downPullObject: {
        displayText: "",
        hintText: this.$t("ScrollerCOM.a1"),
        releaseText: this.$t("ScrollerCOM.a2"),
        loadText: this.$t("ScrollerCOM.a3"),
        backgroundColor: "lightgrey",
        fontColor: "black",
        fontSize: 16
      }, //下拉文案
      isDownPull: false //判断是否为下拉行为
    };
  },
  props: {
    // 是否上拉
    isDownPullEvent: {
      type: Boolean,
      default: true
    },
    // 导航高度
    navHeight: {
      type: [Number, String],
      default: 100
    },
    // 下拉提示文本
    downPullHintText: {
      type: String,
      default: ""
    },
    // 下拉释放文本
    downPullReleaseText: {
      type: String,
      default: ""
    },
    // 下拉加载文本
    downPullLoadText: {
      type: String,
      default: ""
    },
    // 下拉背景颜色
    downPullLoadText: {
      type: [Number, String],
      default: ""
    },
    // 下拉字体颜色
    downPullFontColor: {
      type: [Number, String],
      default: ""
    },
    // 下拉字体大小
    downPullFontSize: {
      type: [Number, String],
      default: 14
    }
  },
  created() {
    if (this.downPullHintText != null) {
      this.downPullObject.hintText = this.downPullHintText;
      console.log(this.downPullObject.hintText);
    }
    if (this.downPullReleaseText != null) {
      this.downPullObject.releaseText = this.downPullReleaseText;
    }
    if (this.downPullLoadText != null) {
      this.downPullObject.loadText = this.downPullLoadText;
    }
    if (this.downPullBackgroundColor != null) {
      this.downPullObject.backgroundColor = this.downPullBackgroundColor;
    }
    if (this.downPullFontColor != null) {
      this.downPullObject.fontColor = this.downPullFontColor;
    }
    if (this.downPullFontSize != null) {
      this.downPullObject.fontSize = this.downPullFontSize;
    }
    // 判断是否来源客户端
    if (process.client) {
      this.$nextTick(function() {
        window.addEventListener("scroll", this.onScroll);
      });
    }
  },
  methods: {
    onScroll() {
      var scrollTop =
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop ||
        window.screen.height ||
        window.screen.availHeight;
      //变量windowHeight是可视区的高度
      var windowHeight =
        document.documentElement.clientHeight || document.body.clientHeight;
      //变量scrollHeight是滚动条的总高度
      var scrollHeight =
        document.documentElement.scrollHeight || document.body.scrollHeight;
      //滚动条到底部的条件

      if (scrollTop + windowHeight + this.navHeight >= scrollHeight) {
        //触底
        this.$emit("upPullEvent");
      }
    },
    // scroll(e) {
    // 	console.log("sdas")
    // 	if (this.$refs.content.scrollTop + this.$refs.content.clientHeight == this.$refs.content.scrollHeight) { //触底
    // 		this.$emit('upPullEvent')
    // 	}
    // },
    startTouch(e) {
      this.$refs.enclose.style.transition = "none"; //点下不需要过度效果，取消掉
      this.$refs.down.style.transition = "none"; //点下不需要过度效果，取消掉
      this.originY = e.targetTouches[0].clientY; //手指按下记录一个起点   用于判断上滑下滑
      this.originY2 = e.targetTouches[0].clientY; //手指按下记录一个起点
    },
    move(e) {
      if (!this.isDownPullEvent) return;
      this.targetY = e.targetTouches[0].clientY; //用户手指实时滑动坐标，配合originY来判断用户上滑还是下滑
      // 下拉
      if (this.originY - this.targetY < 0) {
        this.isDownPull = true;
        if (this.$refs.content.scrollTop == 0) {
          // e.preventDefault();
          this.updataOriginY = e.targetTouches[0].clientY;
          //会有抖动,先隐藏
          // this.top =
          //   (this.updataOriginY - this.originY2) / 2.0 + this.historyTop;
          this.downPullObject.displayText =
            this.top >= 70
              ? this.downPullObject.releaseText
              : this.downPullObject.hintText;
        } else {
          this.originY2 = e.targetTouches[0].clientY; //不是顶部的时候反复刷新起点
        }
      }
      // 上拉
      if (this.originY - this.targetY > 0) {
        if (this.top > 0) {
          e.preventDefault();
          if (this.isDownPull) {
            //判断上滑下滑动作时候连贯
            this.top = (this.targetY - this.originY2) / 2.0 + this.historyTop;
          } else {
            this.top = this.historyTop - (this.originY2 - this.targetY);
          }
        }
      }
      this.originY = e.targetTouches[0].clientY; //重新定义起点为上一步滑动的位置。这样才能实时判断用户上滑还是下滑
    },
    endTouch() {
      this.$refs.enclose.style.transition = "all .3s ease-in-out"; //释放添加回弹效果
      this.$refs.down.style.transition = "all .3s ease-in-out"; //释放添加回弹效果
      this.isDownPull = false;
      if (this.top >= 70) {
        this.downPullObject.displayText = this.downPullObject.loadText;
        this.top = 70;
        setTimeout(() => {
          if (document.documentElement.scrollTop == 0) {
            this.$emit("downPullEvent");
          }
          if (this.top <= 0) {
            return;
          }
          this.top = 0;
          this.historyTop = 0;
          this.downPullObject.displayText = this.downPullObject.loadText;
          this.$refs.enclose.style.transition = "all .2s ease-in-out"; //释放添加回弹效果
          this.$refs.down.style.transition = "all .2s ease-in-out"; //释放添加回弹效果
          this.isDownPull = false;
        }, 500);
      } else {
        this.downPullObject.displayText = this.downPullObject.hintText;
        this.top = 0;
      }
      this.historyTop = this.top;
      return;
      //  这个基于手机端的触底
      if (
        this.$refs.content.scrollTop + this.$refs.content.clientHeight >=
        this.$refs.content.scrollHeight
      ) {
        //触底
        this.$emit("upPullEvent");
      }
    }
  }
};
</script>

<style scoped>
.operationBox .content {
  height: 100%;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
  /* border: 1px solid #000; */
  position: relative;
}
.operationBox .enclose {
  position: relative;
  top: 0px;
}
.operationBox .down {
  position: absolute;
  width: 100%;
  overflow: hidden;
  padding-bottom: 7px;
  transform: translateY(-7.2px);
  text-align: center;
}
</style>
