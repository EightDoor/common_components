<template>
    <zk-comm-scroll title="tabs">
        
        <view class="content">
            <slot/>
        </view>
    </zk-comm-scroll>
</template>
<script lang="ts">
import {defineComponent, ref, } from 'vue';
import log from '../../utils/log';

export default defineComponent({
  name: "ZkCommTabs",
  props: {
    list: {
      type: Array ,
      default: [],
      required: true,
    },
    activeColor: {
      type: String,
      default: "#4cd964"
    },
    // button text
    styleType: {
      type: String,
      default: "text"
    }
  },  
  emits: ["changeIndex"],
  setup(props, {emit}) {
    const current = ref(0)
    function onClickItem(e: any) {
      const currentIndex = e.currentIndex
      emit("changeIndex", currentIndex)
    }
    return {
      onClickItem,
      current
    }
  }
})
</script>
<style lang="scss" scoped>
  .segmented {
    width: 100vw;
    overflow-x: scroll;
    border: 1px solid red;
  }
</style>