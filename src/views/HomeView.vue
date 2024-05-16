<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue'
import MapImg from '../components/MapImg.vue'
import { onMounted, h, render, ref } from 'vue'
import BaiduMap from '@/components/baiduMap.vue'
import MapOverlay from '@/components/mapOverlay.vue'
let list = ref([[116.40342230333138, 39.92498414216742]])
onMounted(() => {
  // render(h(MapImg), document.getElementById('container'))
  setTimeout(()=>{
    list.value = [[117.40342230333138, 39.92498414216742]]
    setTimeout(()=>{
      list.value = [[116.40342230333138, 39.92498414216742]]
      setTimeout(()=>{
        console.log("销毁啦")
        list.value = []
      },1000)
    },3000)
  },2000)

})
</script>

<template>
  <baiduMap  :point="[116.404, 39.915]">
      <template v-if="list.length">
        <mapOverlay v-for="(itm,index) in list" :point="itm" :key="index"></mapOverlay>
      </template>

  </baiduMap>
</template>
<style>
#container {
  width: 100%;
  height: 100%;
}
</style>
