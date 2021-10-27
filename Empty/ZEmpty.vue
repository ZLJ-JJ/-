<!--
 * @Author: zlj
 * @Date: 2020-09-09 09:57:10
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2021-09-08 15:44:29
 * @Description: 
 * @FilePath: \reconsitution-app\compoents\noSearGood.vue
-->
<template>
  <div class="no-sear-good">
    <div class="no-sear-good-img flex_col_center2">
      <img :src="imgSrc" width="134px" height="113px" />
      <div v-if="goodFlag">{{ describe }}</div>
      <div v-else>
        {{ $t("ZEmptyCOM.a1") }} <br />
        {{ $t("ZEmptyCOM.a2") }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgSrc: {
      type: String,
      default: "",
    },
    describe: {
      type: String,
      default: "",
    },
    goodFlag: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {};
  },
  computed: {},
  created() {},
  mounted() {},
  watch: {},
  methods: {},
  components: {},
};
</script>

<style scoped lang="less">
.no-sear-good-img {
  width: 100%;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  text-align: center;
  background: #f8f8f8;
  div {
    margin-top: 29px;
    font-size: 30px;
    color: #666;
  }
}
</style>
