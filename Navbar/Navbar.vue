<!--
 * @Author: your name
 * @Date: 2021-08-05 20:16:04
 * @LastEditTime: 2021-09-06 17:18:31
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\navbar\navbar.vue
-->
<template>
  <div :class="{ is_open: isOpen && !isApp }">
    <div class="open-title flex_space flex" v-if="isOpen && !isApp">
      <img src="@/assets/img/logo-icon.png" class="logo-icon" />
      <div class="w-400">
        <div class="fs-24">{{ $t("supplementPG.txt62") }}</div>
        <div class="fs-22 c-999">
          {{ $t("supplementPG.txt63") }}
        </div>
      </div>
      <div class="open-btn flex_center2" @click="openApp">
        {{ $t("supplementPG.txt61") }}
      </div>
    </div>
    <div class="balance-header" :class="{ is_fixed: isFixed }">
      <div v-if="isBack">
        <van-icon name="arrow-left" class="arrow-left" @click="toBack" />
      </div>
      <!-- 多标题 -->
      <div v-if="title2.length > 1" class="flex content_txt">
        <div
          v-for="(item, index) in title2"
          :key="index"
          class="header-t1"
          :class="{ fw_400: txtSize == index }"
          @click="titleIndex(index)"
        >
          {{ item.name }}
        </div>
      </div>
      <!-- 单标题 -->
      <span class="header-title" v-if="title">{{ title }}</span>
      <!-- 搜索框 -->
      <section
        class="header-t1-nav pd_t_10"
        @click="title2Click"
        :style="{ display: navShow }"
      >
        <slot name="navSearch"></slot>
      </section>
      <div class="header-t2 c-orange" @click="title2Click">
        <slot name="navRight"></slot>
      </div>
      <div class="nav_bottom">
        <slot name="navBottom"></slot>
      </div>
    </div>
    <Zhezhao v-show="modalShow" :isCenter="true">
      <div class="cancal_follow">
        <div class="content_txt">
          <p class="flex_center title">{{ $t("supplementPG.txt59") }}</p>
        </div>
        <div class="foot_btn flex">
          <span class="btn lan flex flex_center">{{
            $t("supplementPG.txt60")
          }}</span>
          <span class="btn bai flex flex_center">{{
            $t("supplementPG.txt61")
          }}</span>
        </div>
      </div>
    </Zhezhao>
  </div>
</template>

<script>
export default {
  props: {
    // 标题
    title: {
      type: String,
      default: ""
    },
    title2: {
      type: String,
      default: ""
    },
    txtSize: {
      type: Number,
      default: 0
    },
    // 返回
    isBack: {
      type: Boolean,
      default: true
    },
    // 跳转
    jumpName: {
      type: String,
      default: ""
    },
    // 是否定位
    isFixed: {
      type: Boolean,
      default: false
    },
    //是否显示打开APP
    isOpen: {
      type: Boolean,
      default: false
    },
    // 多个标题
    title2: {
      type: [Array, Object],
      default: () => []
    },
    // toBack: {
    //   type: Function,
    //   default: () => {
    //     window.history.back(-1);
    //   }
    // },
    // 显示隐藏
    navShow: {
      type: String,
      default: "inline-block"
    },
    //是否需要自定义返回方法
    toBackFun: {
      type: Boolean,
      default: false
    },
    //什么类型的页面需要唤醒APP  2清仓 3商品详情
    itemType: {
      type: Number,
      default: 2
    }
  },
  data() {
    return {
      isApp: null,
      modalShow: false
    };
  },
  computed: {},
  created() {},
  mounted() {
    let NT = navigator.userAgent;
    if (NT.indexOf("tospino") != -1) {
      this.isApp = true;
      this.isOpen = false;
    }
  },
  watch: {},
  methods: {
    title2Click() {
      if (this.jumpName == "") return;
      this.$emit("jumpRouter", this.jumpName);
    },
    // 多个标题
    titleIndex(idx) {
      this.$emit("titleArrIndex", idx);
    },
    toBack() {
      if (this.toBackFun) {
        this.$emit("showTips");
      } else {
        if (this.isApp) {
          javascript: webCtrl.goBack();
          window.history.back(-1);
        } else {
          window.history.back(-1);
        }
      }
    },
    openApp() {
      this.appCode = navigator.userAgent;
      var isIOS = !!this.appCode.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
      if (this.appCode.indexOf(";tospino") != -1) {
        this.isApp = true;
      } else {
        this.isApp = false;
      }
      //android端
      if (!this.isApp) {
        //安卓app的scheme协议
        window.location.href = `tospinomarket://platform:8888/from?type=url&url=${window.location.pathname}`;
        setTimeout(function() {
          let hidden =
            window.document.hidden ||
            window.document.mozHidden ||
            window.document.msHidden ||
            window.document.webkitHidden;
          if (typeof hidden == "undefined" || hidden == false) {
            //下载地址
            window.location.href =
              "https://play.google.com/store/apps/details?id=uni.UNIEABBF4E&hl=zh&gl=US";
          }
        }, 2000);
      }
      //ios端
      if (isIOS) {
        //ios的scheme协议
        switch (this.itemType) {
          case 2:
            window.location.href = `iOStospinoMarket://type=2&skuid=`;
            break;
          case 3:
            window.location.href = `iOStospinoMarket://type=3&skuid=${this.$route.query.skuId}`;
            break;
        }

        setTimeout(function() {
          let hidden =
            window.document.hidden ||
            window.document.mozHidden ||
            window.document.msHidden ||
            window.document.webkitHidden;
          console.log();
          if (typeof hidden == "undefined" || hidden == false) {
            //App store下载地址
            window.location.href =
              "https://itunes.apple.com/cn/app/id1548183531?mt=8";
          }
        }, 2000);
      }
    }
  },
  components: {}
};
</script>

<style scoped lang="less">
.is_fixed {
  position: fixed;
  top: 0;
  z-index: 99;
}
.open-title {
  position: fixed;
  top: 0;
  z-index: 99;
  width: 100vw;
  height: 136px;
  padding: 0 32px;
  background-color: #fff;
  .logo-icon {
    width: 88px;
    height: 88px;
    margin-right: 32px;
  }
  .w-400 {
    width: 400px;
  }
  .open-btn {
    width: 136px;
    height: 72px;
    background: #2f80ed;
    border-radius: 100px;
    font-size: 28px;
    color: #fff;
  }
}
.balance-header {
  width: 100%;
  height: 88px;
  background-color: #fff;
  text-align: center;
  // position: relative;
  border-bottom: 1px solid #e8e8e8;
  line-height: 84px;
  .arrow-left {
    position: absolute;
    top: 20px;
    left: 30px;
    font-size: 40px;
  }
  .header-t1 {
    display: inline-block;
    line-height: 84px;
    font-size: 36px;
    color: #333;
    font-weight: bold;
  }
  .header-t1-nav {
    display: inline-block;
    line-height: 84px;
    font-size: 36px;
    color: #333;
    font-weight: bold;
    position: absolute;
    left: 86px;
  }
  .fw_400 {
    font-weight: 400;
  }
  .header-title {
    font-size: 36px;
    color: #333;
    font-weight: bold;
    // margin-right: 88px;
  }
  .header-t1:first-child {
    margin-right: 88px;
  }
  .header-t2 {
    position: absolute;
    right: 36px;
    top: 22px;
    font-size: 32px;
    font-weight: 400;
    // color: #0e66de;
  }
  .content_txt {
    margin-left: 204px;
    .t1 {
      margin-left: 88px;
    }
  }
  .nav_bottom {
    position: fixed;
    top: 84px;
    width: 100%;
  }
}
.cancal_follow {
  background-color: #fff;
  width: 600px;
  padding: 48px 70px 40px;
  border-radius: 12px;
  .title {
    font-size: 32px;
    color: #303133;
    font-weight: 900;
    margin: 40px 0;
  }
  .btn {
    width: 212px;
    height: 64px;
    border: 2px solid #2f80ed;
    text-align: center;
    border-radius: 40px;
    font-size: 28px;
  }
  .lan {
    color: #0e66de;
  }
  .bai {
    background-color: #2f80ed;
    color: #fff;
    margin-left: 70px;
  }
}
.is_open {
  .nav_bottom {
    top: 208px !important;
  }
  .is_fixed {
    top: 134px;
  }
}
</style>
