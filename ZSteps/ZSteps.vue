<template>
  <div class="steps">
    <div class="tail active-tail"></div>
    <div class="node active-node"></div>
    <div class="wrapper">
      <div class="title">
        <span>{{ item[wrapperStatus] }}</span>
        <slot name="title"></slot>
      </div>
      <div class="wrapperTitle">
        <span>{{ item[wrapperTitle] }}</span>
        <slot name="content"></slot>
      </div>
      <div class="time">
        <slot name="time"></slot>
      </div>
    </div>
  </div>
</template>

<script>
/*
	item：没条记录详情
	activity: 当前进度条状态
	wrapperStatus：流程状态对应字段
	wrapperTitle：详情对应字段
	index：进度条排列序号
	date：时间
	 */
export default {
  name: "ZSteps",
  data() {
    return {};
  },
  props: {
    item: {
      type: Object,
    },
    activity: {
      type: Number,
      default: 0,
    },
    wrapperStatus: {
      type: String,
      default: "status",
    },
    wrapperTitle: {
      type: String,
      default: "content",
    },
    date: {
      type: String,
      default: "date",
    },
    index: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style lang="less" scoped>
.steps {
  position: relative;
  padding-bottom: 40px;

  &:last-child {
    .tail {
      display: none;
    }
  }

  .date {
    position: absolute;
    top: -6px;
    color: #aaaaaa;
    text-align: center;
  }

  .active-tail {
    border-left: 2px solid #2f80ed !important;
  }

  .active-node {
    background-color: #2f80ed !important;
  }

  .tail {
    position: absolute;
    left: 10px;
    height: 100%;
    border-left: 2px solid #e4e7ed;
  }

  .node {
    left: 0px;
    width: 24px;
    height: 24px;
    position: absolute;
    background-color: #e4e7ed;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .wrapper {
    position: relative;
    padding-left: 40px;
    top: -6px;
    .title {
      font-size: 30px;
      color: #333;
    }
    .wrapperTitle {
      font-size: 28px;
      color: #666;
      margin: 10px 0;
    }
    .time {
      font-size: 26px;
      color: #999;
    }
  }
}
</style>
