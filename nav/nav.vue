<template>
  <van-nav-bar
    :title="title"
    @click-left="$router.go(-1)"
    left-arrow
    fixed
    :placeholder="true"
  />
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: ""
    }
  }
};
</script>

<style lang="less" scoped>
.van-nav-bar {
  height: 88px;
  line-height: 88px;
  /deep/ .van-nav-bar__content {
    height: 88px !important;
    line-height: 88px !important;
    .van-nav-bar__left {
      .van-nav-bar__arrow {
        font-size: 40px;
        color: #333;
      }
    }
    .van-nav-bar__title {
      font-size: 36px;
    }
  }
}
</style>
