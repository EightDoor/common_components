<template>
   <zk-comm-scroll title="首页"
   >
    <view>
      <text  
        v-for="(item, index) in list"
        :key="index" 
        @click="goLoadMore(item.path)"
        class="li"
      >
        <span>{{item.title}}</span>
        <span class="jump">点击跳转</span>
      </text>
    </view>
    </zk-comm-scroll>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import log from '../../uni_modules/zk-comm/utils/log';

export default defineComponent({
  name: 'PagesIndex',
  setup() {
    const list = [
      {
        title: '加载更多',
        path: '/pages/demo/load_more',
      },
      {
         title: 'tabs',
        path: '/pages/demo/tabs',
      }
    ];
    function goLoadMore(url: string) {
      log.d(url, 'url');
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
