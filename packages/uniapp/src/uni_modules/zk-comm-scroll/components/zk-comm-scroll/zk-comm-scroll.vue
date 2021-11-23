<template>
  <view>
    <nav-bar :title="title" :isLeft="isLeft">
      <slot name="navBarRight" />
    </nav-bar>
    <custom-content
      :refresh="refresh"
      @loadMore="loadMore"
      @onScroll="onScroll"
      @goTop="goTop"
    >
      <slot />
    </custom-content>
  </view>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import NavBar from "../zk-comm-navbar/zk-comm-navbar.vue";
import CustomContent from "../zk-comm-content/zk-comm-content.vue";
import log from "../../utils/log";
import { CallLoadMoreType } from "../../types";

export default defineComponent({
  name: "ComIndex",
  components: {
    NavBar,
    CustomContent,
  },
  props: {
    title: {
      type: String,
      default: "",
    },
    isLeft: {
      type: Boolean,
      default: false,
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
  emits: ["refresh", "loadMore", "update:page", "update:size", "onScroll", "goTop"],
  setup(props, { emit }) {
    const pageNum = ref(1);
    const pageSize = ref(10);

    function loadMore(data: CallLoadMoreType) {
      emit("loadMore", data);
    }

    function updatePage(val: number) {
      emit("update:page", val);
    }

    function updateSize(val: number) {
      emit("update:size", val);
    }

    function onScroll(e: any) {
      emit("onScroll", e);
    }

    function goTop() {
      emit("goTop");
    }

    return {
      pageNum,
      pageSize,
      loadMore,
      updatePage,
      updateSize,
      onScroll,
      goTop,
    };
  },
});
</script>
<style lang="scss" scoped>
// var(--status-bar-height)
$navBarHeight: 20px;
$tabBarHeight: 50px;
.content_container {
  padding: 15px;
}
.scroll_view {
  height: calc(100vh - $navBarHeight - $tabBarHeight);
}
</style>
