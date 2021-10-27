<!--
 * @Author: your name
 * @Date: 2021-08-17 11:50:03
 * @LastEditTime: 2021-08-17 11:50:25
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \b2b-vietnam-i18n\components\B2Bcomm\ZSwiper\ZSwiperitem.vue
-->
<template>
  <div class="slide">
    <!-- 组件插槽给图片站的位置 -->
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Slide",
};
</script>

<style scoped>
.slide {
  width: 100%;
  flex-shrink: 0;
}

.slide img {
  width: 100%;
}
</style>
