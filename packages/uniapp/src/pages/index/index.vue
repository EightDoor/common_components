<template>
  <zk-comm-scroll title="首页">
    <view>
      <view
        v-for="(item, index) in list"
        :key="index"
        @click="goLoadMore(item.path)"
        class="li"
      >
        <text>{{ item.title }}</text>
        <text class="jump">点击跳转</text>
      </view>
    </view>
  </zk-comm-scroll>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import log from "../../uni_modules/zk-comm/utils/log";

export default defineComponent({
  name: "PagesIndex",
  setup() {
    const list = [
      {
        title: "加载更多",
        path: "/pages/demo/load_more",
      },
      {
        title: "tabs简单使用",
        path: "/pages/demo/tabs",
      },
      {
        title: "tabs复杂使用(上拉加载，下拉刷新)",
        path: "/pages/demo/tabs_complex",
      },
    ];
    function goLoadMore(url: string) {
      log.d(url, "url");
      uni.navigateTo({
        url,
      });
    }
    return {
      goLoadMore,
      list,
    };
  },
});
</script>

<style lang="scss" scoped>
.jump {
  font-size: 13px;
  color: #ccc;
}
.li {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
