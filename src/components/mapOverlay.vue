<script setup lang="ts">
import { h, inject,  onBeforeUnmount, render, watchEffect } from 'vue'
import MapImg from '@/components/MapImg.vue'
const props = defineProps({
  point:{
    type:Array,
    default(){
      return [116.40342230333138, 39.92498414216742]
    }
  }
})
const map :any= inject('map')
let customOverlay :any;
/*
* 创建自定义覆盖物DOM
* */
watchEffect(()=>{
  if(!map.value){
    return
  }
  // 创建
  if(!customOverlay){
   customOverlay = new window.BMapGL.CustomOverlay(createDOM2, {
    point: new window.BMapGL.Point(props.point[0], props.point[1]),
    opacity: 0.5,
    map: map,
    offsetY: -10,
    properties: {
      title: 'this is a title',
      size: '100'
    }
  });
   // 更新
    map.value.addOverlay(customOverlay);
    return
  }
  if(props.point?.length===2){
    customOverlay.setPoint(new window.BMapGL.Point(props.point[0], props.point[1]))
  }
})
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
onBeforeUnmount(()=>{
  console.log("销毁啦")
  render(null,customOverlay?.domElement?.firstChild)
  map.value.removeOverlay(customOverlay)
})
</script>
<style scoped>

</style>
