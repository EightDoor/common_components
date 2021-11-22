<template>
  <zk-comm-scroll
    :refresh="refresh"
    @refresh="callRefres"
    :page="page"
    :size="size"
    @update:page="updatePage"
    @update:size="updateSize"
    @loadMore="loadMore"
    @onScroll="onScroll"
    :title="title"
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
    <view class="content">
      <slot />
    </view>
  </zk-comm-scroll>
</template>
<script lang="ts">
import { toPlainObject } from "lodash";
import { defineComponent, ref } from "vue";
import log from "../../utils/log";

export default defineComponent({
  name: "ZkCommTabs",
  props: {
    title: {
      type: String,
      default: "",
    },
    list: {
      type: Array,
      default: [],
      required: true,
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
    refresh: {
      type: Boolean,
      default: false,
    },
    page: {
      type: Number,
      default: 1,
    },
    size: {
      type: Number,
      default: 10,
    },
  },
  emits: ["changeIndex", "refresh", "loadMore", "update:page", "update:size"],
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

    // 上拉加载、下拉刷新
    const pageNum = ref(1);
    const pageSize = ref(10);

    function callRefres(done: Function) {
      emit("refresh", () => {
        done();
      });
    }

    function loadMore(done: Function) {
      emit("loadMore", (val: any[]) => {
        done(val);
      });
    }

    function updatePage(val: number) {
      emit("update:page", val);
    }

    function updateSize(val: number) {
      emit("update:size", val);
    }
    return {
      current,
      defaultTextColor,
      change,
      onScroll,
      isCeiling,

      callRefres,
      pageNum,
      pageSize,
      loadMore,
      updatePage,
      updateSize,
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
  position: fixed;
  top: 65px;
  left: 0;
  background: white;
  padding: 10px 0;
}
</style>
