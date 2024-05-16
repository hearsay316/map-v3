<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

function handleClick(e){
  console.log(e,'sss')
}
onMounted(() => {
  console.log('onMounted')
})
onUnmounted(() => {
  console.log('onUnmounted')
})
</script>

<template>

  <img @click="handleClick" style="width: 80px; height: 240px;" src='https://bj.bcebos.com/v1/mapopen-pub-jsapigl/assets/images/fire.gif' alt="">

</template>

<style scoped>

</style>
