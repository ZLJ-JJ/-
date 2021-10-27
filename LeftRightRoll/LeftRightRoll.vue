<template>
  <div class="scrolllinkage-container" :style="{ top: rollTop + 'px' }">
    <div class="content">
      <div
        class="left-wrap u-tab-div"
        ref="leftWarp"
        :style="{ width: leftWidth + 'px' }"
      >
        <div>
          <div
            class="left-item u-tab-item"
            v-for="(item, index) in leftItemList"
            :key="index"
            :class="[current == index ? 'u-tab-item-active' : '']"
            @click="
              selectMenu(index, $event, item.categoryId, item.classNameEng)
            "
            ref="leftItemList"
          >
            <span class="sider_title">{{ item.classNameEng }}</span>
          </div>
        </div>
      </div>
      <div
        class="right-wrap"
        :style="{ top: rollTop + 'px', background: rollBackground }"
        ref="rightWarp"
      >
        <div>
          <!-- 不联动 -->
          <div>
            <slot name="z-roll-right"></slot>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 左侧栏
    leftItemList: {
      type: Array,
      default: []
    },
    // 右侧栏联动
    rightItemList: {
      type: Array,
      default: []
    },
    // 左侧栏联动
    leftWidth: {
      type: [Number, String],
      default: 94
    },
    // 联动距离顶部 距离
    rollTop: {
      type: [Number, String],
      default: 0
    },
    // 联动距离顶部 距离
    rollBackground: {
      type: [String],
      default: "#f8f8f8"
    }
  },
  data() {
    return {
      current: 0 // 预设当前项的值
    };
  },
  computed: {},
  mounted() {},
  methods: {
    async selectMenu(index, event, id, sname) {
      this.current = index;
      this.$emit("swichMenu", index, id, sname);
    }
  }
};
</script>

<style scoped lang="less">
.sider_title {
  text-align: center;
}

.scrolllinkage-container {
  position: absolute;
  left: 0;
  height: 100%;
  width: 100%;

  // .header {
  //   width: 100%;
  //   height: 40px;
  //   background: rgba(7, 17, 27, 0.2);
  //   line-height: 40px;
  //   color: #444340;
  //   font-weight: 600;
  //   text-align: center;
  // }
  .content {
    display: flex;
    position: absolute;
    top: 104px;
    bottom: 110px;
    width: 100%;
    background: #fff;
    overflow: hidden;

    .left-wrap {
      flex: 0 1 auto;
      height: 100%;
      background: #fff;
      overflow: scroll;
      &::-webkit-scrollbar {
        width: 0;
      }
      .left-item {
        width: 100%;
        height: 110px;
        line-height: 54px;
        text-align: center;
        background: #fff;
        font-size: 24px;
        color: #343434;
        font-weight: 500;
        line-height: 1;
        margin-bottom: 2px;
        border-bottom: 2px solid #f8f8f8;
        box-sizing: border-box;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0 10px;
      }
      .u-tab-item-active {
        position: relative;
        color: #2f80ed;
        font-size: 24px;
        font-weight: bold;
        padding: 0 10px;
        ::before {
          content: "";
          position: absolute;
          border-left: 6px solid #2f80ed;
          height: 40px;
          left: 0;
          top: 39px;
        }
      }
      .current {
        background: #fff;
      }
    }
    .right-wrap {
      flex: 1;
      overflow: scroll;
      .right-item {
        .title {
          padding-left: 14px;
          height: 26px;
          line-height: 26px;
          border-left: 2px solid #d9dde1;
          font-size: 12px;
          color: #93999f;
          background: #f3f5f7;
        }
        .detail {
          margin-top: 10px;
          .detail-content {
            height: 50px;
            padding-left: 10px;
            line-height: 50px;
            color: #93999f;
            font-size: 20px;
          }
        }
      }
    }
  }
}
</style>
