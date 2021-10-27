<!--
 * @Author: zlj
 * @Date: 2021-07-16 15:36:55
 * @LastEditTime: 2021-09-08 16:47:21
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-h5\components\B2Bcomm\footer.vue
-->
<template>
  <!-- 底部栏 -->
  <div class="footer flex_space flex_center2">
    <div
      v-for="(item, index) in tabbar"
      :key="index"
      @click="tabsUrl(item.url)"
    >
      <!-- :to="switchLocalePath(locale.code)" -->
      <nuxt-link :to="item.url">
        <div class="item_footer flex_col_center2">
          <div>
            <img
              class="footer-icon-style"
              :src="$route.path == item.url ? item.imgSrcActive : item.imgSrc"
            />
          </div>
          <div :class="{ active: item.url == $route.path }">
            {{ item.name }}
          </div>
        </div>
      </nuxt-link>
    </div>
  </div>
</template>
<
<script>
import home from "@/assets/img/b2-app/home.png";
import sort from "@/assets/img/b2-app/sort.png";
import cart from "@/assets/img/b2-app/cart.png";
import mine from "@/assets/img/b2-app/mine.png";

import home1 from "@/assets/img/b2-app/home1.png";
import sort1 from "@/assets/img/b2-app/sort1.png";
import cart1 from "@/assets/img/b2-app/cart1.png";
import mine1 from "@/assets/img/b2-app/mine1.png";
export default {
  created() {
    // 登陆时  默认首页
    const _pagePath = this.$route.path;
    this.$i18n.locales.forEach(lang => {
      if (_pagePath == "/" + lang.code) {
        this.tabbar[0].url = "/" + lang.code;
      }
    });
  },
  data() {
    return {
      path: "",
      tabbar: [
        {
          imgSrc: home,
          imgSrcActive: home1,
          name: this.$t("supplementPG.txt1"),
          url: "/"
        },
        {
          imgSrc: sort,
          imgSrcActive: sort1,
          name: this.$t("supplementPG.txt2"),
          url: "/classify"
        },
        {
          imgSrc: cart,
          imgSrcActive: cart1,
          name: this.$t("supplementPG.txt3"),
          url: "/shoppingCart"
        },
        {
          imgSrc: mine,
          imgSrcActive: mine1,
          name: this.$t("supplementPG.txt4"),
          url: "/account"
        }
      ]
    };
  },
  mounted() {},
  methods: {
    tabsUrl(url) {
      this.$router.push({ path: url });
      //  this.$router.push(idx);
    }
  }
};
</script>
<style lang="less">
.footer {
  z-index: 999 !important;
  position: fixed;
  bottom: 0;
  width: 750px;
  height: 120px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50%;
  background: #ffffff;
  border-top: 1px solid #eeeeee;
  font-weight: bold;
}

.item_footer {
  width: 116px;
}

.footer-icon-style {
  width: 44px;
  height: 44px;
  margin-bottom: 12px;
}
.active {
  color: rgb(14, 102, 222);
}
</style>
