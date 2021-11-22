<template>
  <view>
    <zk-comm-tabs :list="list" @changeIndex="changeIndex">
      <text v-if="current === 0">1</text>
      <text v-if="current === 1">123</text>
    </zk-comm-tabs>
  </view>
</template>
<script lang="ts">
import {defineComponent, ref, watch} from 'vue';

export default defineComponent({
  name: "DemoTabs",
  setup() {
    const list = ref(["第一项", '第二项', "第一项", "第一项","第一项","第一项", "第一项", "第一项", "第一项"]);
    const current = ref(0);

    function changeIndex(val: number) {
      current.value = val;
    }
    return {
      list,
      current,
      changeIndex
    }
  }
})
</script>