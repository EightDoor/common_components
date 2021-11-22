<template>
  <view>
    <zk-comm-tabs :list="list" title="tabs" @changeIndex="changeIndex">
      <view class="test" v-show="current === 0">
        <text>12</text>
      </view>
      <text v-show="current === 1">123</text>
    </zk-comm-tabs>
  </view>
</template>
<script lang="ts">
import { defineComponent, ref, watch } from "vue";

export default defineComponent({
  name: "DemoTabs",
  setup() {
    const list = ref([
      "第一",
      "第二",
      "第一",
      "第一",
      "第一",
      "第一项",
      "第一项",
      "第一项",
      "第一项",
    ]);
    const current = ref(0);

    function changeIndex(val: number) {
      current.value = val;
    }
    return {
      list,
      current,
      changeIndex,
    };
  },
});
</script>
<style scoped lang="scss">
.test {
  height: 1500px;
  background: white;
}
</style>
