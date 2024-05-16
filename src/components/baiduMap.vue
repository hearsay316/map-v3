<script setup lang="ts">
import MapImg from '../components/MapImg.vue'
import { onMounted, h, render, ref, provide } from 'vue'
const container = ref()
const map = ref()
defineOptions({
  name: 'BaiduMap'
})
const props =  defineProps({
  point: {
    type: Array,
    default: () => {
      return [116.404, 39.915]
    }
  },
  zoom: {
    type: [Number,String],
    default: 15
  }
})
onMounted(() => {
  setTimeout(()=>{
    map.value = new window.BMapGL.Map(container.value);
    const point = new window.BMapGL.Point(props.point[0], props.point[1]);
    map.value.centerAndZoom(point, props.zoom);
  })
//   var point = new window.BMapGL.Point(116.404, 39.915);  // 创建点坐标
//   map.centerAndZoom(point, 15);
//   const customOverlay = new window.BMapGL.CustomOverlay(createDOM2, {
//     point: new window.BMapGL.Point(116.40342230333138, 39.92498414216742),
//     opacity: 0.5,
//     map: map,
//     offsetY: -10,
//     properties: {
//       title: 'this is a title',
//       size: '100'
//     }
//   });
//   console.log(customOverlay,'customOverlay')
// // 将自定义覆盖物添加到地图上
//   map.value.addOverlay(customOverlay);
  // setTimeout(()=>{
  //   render(null,customOverlay.domElement.firstChild)
  //   map.removeOverlay(customOverlay)
  // },10000)
})
// 创建自定义覆盖物DOM
function createDOM() {
  const img = document.createElement('img');
  img.style.height = '240px';
  img.style.width = '80px';
  img.src = 'https://bj.bcebos.com/v1/mapopen-pub-jsapigl/assets/images/fire.gif';
  img.draggable = false;
  return img;
}

// 创建自定义覆盖物DOM
function createDOM2() {
  const vNode = h(MapImg)
  // 实例化组件
  let dom2 = document.createElement('div');
  render(vNode,dom2)
  // 初始化地图，设置中心点坐标和地图级别
  // 创建自定义覆盖物
  return dom2;
}
provide('map', map)
defineExpose({
  map: map
})
</script>

<template>
  <main ref="container" class="container" id="container">
  </main>
  <slot></slot>
</template>
<style>
#container {
  width: 100%;
  height: 100%;
}
</style>
