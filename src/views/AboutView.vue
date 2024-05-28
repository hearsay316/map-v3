<template>
  <div @click="handleClick"  class="about">
    <h1> <span :data-index="index" v-for="(item,index) in str" :key="index">{{item}}</span></h1>
  </div>
  <input style="opacity: 0;" @keydown="handlekeydown" @compositionend="compositionend" @compositionstart="compositionstart" @input="handleInput" ref="inputDom" type="text" />
</template>
<script setup>
import { ref } from 'vue'

let str =ref("This is an about page")
const inputDom = ref(null)
let isCompositing = false // 是否正在输入拼音
let index = ref(null)
function handleClick(e){
  console.log(e,'dedede')
  inputDom.value.focus()
  index.value =  e?.target?.dataset?.index||null
}
function handleInput(e){
  setTimeout(() => {
    let data = e.data
    if (!data || isCompositing) {
      return
    }
    str.value = str.value.slice(0,index.value)+data+str.value.slice(index.value)
    console.log(data.length,data,'data.length')
    index.value=+index.value + data.length
    console.log(  index.value)
    inputDom.value.value = ""
  }, 0)

}
function handlekeydown(e){
  if (e.keyCode === 8) {
    if(index.value===0){
      return
    }
    // delete()
    str.value = str.value.slice(0,index.value-1)+str.value.slice(index.value)
    index.value = index.value-1
  } else if (e.keyCode === 13) {
    // this.newLine()
  }
}
function compositionstart(){
  isCompositing = true // 是否正在输入拼音
  console.log('compositionstart')
}
function compositionend(){
  isCompositing = false
}
</script>
