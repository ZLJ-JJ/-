<!--
 * @Author: your name
 * @Date: 2020-07-20 17:26:48
 * @LastEditTime: 2021-08-16 10:08:19
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \app-en\src\multiplexing\progress.vue
-->
<template>
  <!-- 封装-进度条 -->
  <div>
    <div class="f">
      <div class="progress">
        <div
          :class="{ on_fb: pivotTxt == 1, pre_fb: pivotTxt === 0 }"
          :style="'width:' + parseInt(progressBar) + '%'"
        >
          <div class="text-progress">
            {{
              pivotTxt != 2
                ? parseInt(progressBar) + "%" + $t("ProgressCOM.a1") ||
                  0 + "%" + $t("ProgressCOM.a1")
                : $t("ProgressCOM.a2")
            }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  components: {},
  props: {
    // 进度条显示百分比
    progressBar: {
      type: [Number, String],
      default: 0
    },
    // 状态颜色显示
    pivotTxt: {
      type: [Number, String],
      default: null
    }
  },
  data() {
    return {};
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {}
};
</script>
<style scoped lang="less">
.f {
  //进度条
  padding-top: 10px;
  display: flex;
  line-height: 14px;
  .progress {
    width: 200px;
    height: 32px;
    background: #cccccc;
    border-radius: 15px;
    overflow: hidden;
  }
  .bar {
    width: 1%;
    height: 30px;
    background: rgba(8, 109, 180, 1);
  }
  .text-progress {
    color: #fff;
    width: 232px;
    font-size: 24px;
    text-align: center;
    line-height: 32px;
  }
}
</style>
