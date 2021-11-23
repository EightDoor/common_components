<template>
  <zk-comm-scroll
    :refresh="refresh"
    @loadMore="loadMore"
    @onScroll="onScroll"
    :title="title"
    :isLeft="isLeft"
    @goTop="goTop"
  >
    <scroll-view
      :class="{ tabs_title_container: true, tab_title_ceiling: isCeiling }"
      scroll-x="true"
    >
      <view :class="{ tabs_title_for: list.length <= 4 }">
        <view
          @click="change(index)"
          v-for="(item, index) in list"
          :key="index"
          class="tabs_title"
          :style="{
            width: width,
            borderBottom: `1px solid ${current === index ? activeColor : 'transpart'}`,
          }"
        >
          <text :style="{ color: current === index ? activeColor : defaultTextColor }">
            {{ item }}
          </text>
        </view>
      </view>
    </scroll-view>
    <view>
      <slot />
    </view>
  </zk-comm-scroll>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import { CallLoadMoreType } from "../../types";
import log from "../../utils/log";

export default defineComponent({
  name: "ZkCommTabs",
  props: {
    title: {
      type: String,
      default: "",
    },
    activeColor: {
      type: String,
      default: "#4cd964",
    },
    // button text
    width: {
      type: String,
      default: "50px",
    },
    ceilingHeight: {
      type: Number,
      default: 30,
    },
    isLeft: {
      type: Boolean,
      default: false,
    },
    refresh: {
      type: Boolean,
      default: false,
    },
    list: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  emits: ["changeIndex", "loadMore"],
  setup(props, { emit }) {
    const defaultTextColor = "black";
    const current = ref(0);
    // 是否吸顶
    const isCeiling = ref(false);

    function change(val: number) {
      current.value = val;
      emit("changeIndex", val);
    }

    function onScroll(e: any) {
      const top = e.detail.scrollTop;
      if (top >= props.ceilingHeight) {
        isCeiling.value = true;
      } else {
        isCeiling.value = false;
      }
    }
    function goTop() {
      isCeiling.value = false;
    }

    function loadMore(data: CallLoadMoreType) {
      emit("loadMore", data);
    }

    return {
      current,
      defaultTextColor,
      change,
      onScroll,
      isCeiling,
      goTop,

      loadMore,
    };
  },
});
</script>
<style lang="scss" scoped>
$tabsHeight: 30x;
$tabsPaddingBo: 10px;
.title_base {
  white-space: nowrap;
  margin-bottom: $tabsPaddingBo;
  height: $tabsHeight;
}
.tabs_title_container {
  @extend .title_base;

  .tabs_title_for {
    display: flex;
    flex-direction: row;
    flex: 1;
    justify-content: space-around;
  }
  .tabs_title {
    display: inline-block;
    margin-right: 10px;
    padding-bottom: 5px;
    text-align: center;
  }
}
.tab_title_ceiling {
  @extend .title_base;
  position: fixed !important;
  top: 65px;
  z-index: 10;
  background: white;
  padding: 10px 0;
  animation-duration: 0.5s;
  animation-name: ceilingAnimate;
}
@keyframes ceilingAnimate {
  0% {
    top: 0;
  }
  10% {
    top: 10px;
  }
  20% {
    top: 20px;
  }
  30% {
    top: 30px;
  }
  50% {
    top: 35px;
  }
  70% {
    top: 50px;
  }
  90% {
    top: 60px;
  }
  100% {
    top: 65px;
  }
}
</style>
