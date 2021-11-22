<template>
   <zk-comm-scroll title="加载更多"
    :refresh="true"
    @refresh="refresh"
    :page="page"
    :size="size"
    @update:page="updatePage"
    @update:size="updateSize"
    @load-more="loadMore"
   >
      <view v-for="(item, index) in list" :key="index" style="border: 1px solid red;height: 100px">
        <text>{{item.id}} {{item.title}}</text>
      </view>
    </zk-comm-scroll>
</template>

<script lang="ts">
import {
  defineComponent, ref, onMounted,
} from 'vue';
import { cloneDeep } from 'lodash';

interface List {
  title: string;
  id: number
}
export default defineComponent({
  name: 'PagesLoadMore',
  setup() {
    const page = ref(1);
    const size = ref(10);
    const list = ref<List[]>([]);
    const sourceList = ref<List[]>([]);

    // moack 数据
    function getList() {
      const result: List[] = [];
      for (let i = 0; i < 35; i += 1) {
        result.push({
          title: `当前是第${i}项`,
          id: i,
        });
      }
      sourceList.value = result;
      const v = cloneDeep(result);
      list.value = cloneDeep(v.splice(0, 10));
    }
    onMounted(() => {
      getList();
    });
    // 加载更多
    function loadMore(done: Function) {
      const result = cloneDeep(sourceList.value);
      const v = result.splice((page.value - 1) * size.value, 10);
      setTimeout(() => {
        done(v);
        list.value = list.value.concat(v);
      }, 2000);
    }
    // 刷新
    function refresh(done: Function) {
      setTimeout(() => {
        done();
        const result = cloneDeep(sourceList.value);
        list.value = result.splice(0, 10);
      }, 2000);
    }

    function updatePage(val: number) {
      page.value = val;
    }
    function updateSize(val: number) {
      size.value = val;
    }

    return {
      refresh,
      page,
      size,
      loadMore,
      list,
      updatePage,
      updateSize,
    };
  },
});
</script>

<style>

</style>
