<template>
  <div @click="handleClick" class="about">
    <h1 class=""><span :data-index="index" v-for="(item,index) in str" :key="index">{{ item }}</span></h1>
  </div>
  <input @paste="hanldePaste" style="opacity: 0;" @keydown="handleKeydown" @compositionend="compositionend"
         @compositionstart="compositionstart" @input="handleInput" ref="inputDom" type="text" />
</template>
<script setup>
import { onMounted, ref } from 'vue'

let str = ref(['点', '我', '点', '击输入'])
const inputDom = ref(null)
let cursorEl = null
let isCompositing = false // 是否正在输入拼音
let index = ref(null)
let cursorTimer = null

function handleClick(e) {
  console.log(e, 'eeee')
  if (!cursorEl) {
    cursorEl = document.createElement('div')
    cursorEl.style.position = 'absolute'
    cursorEl.style.width = '1px'
    cursorEl.style.backgroundColor = '#000'
    document.querySelector('.about').appendChild(cursorEl)
  }
  setCursor(e?.target?.dataset?.index || 0)
  inputDom.value.focus()
  index.value = Number(e?.target?.dataset?.index || 0) || null
}

function setCursor(index) {
  setTimeout(() => {
    console.log(index, 'index')
    let e = document.querySelectorAll('.about span')[index]
    if (index < 0) {
      cursorEl.style.left = 0 + 'px'
    }
    if (!e) {
      return
    }
    const { height, width, left, top } = e.getBoundingClientRect()
    console.log(height, width, left, top)
    cursorEl.style.left = left + width + 'px'
    cursorEl.style.top = top + 'px'
    cursorEl.style.height = height + 'px'
    cursorEl.style.opacity = 1
    setTimeout(() => {
      cursorEl.style.display = 'block'
      blinkCursor(0)
    }, 10)
  }, 10)
}

function blinkCursor(opacity) {
  cursorTimer = setTimeout(() => {
    cursorEl.style.opacity = opacity
    blinkCursor(opacity === 0 ? 1 : 0)
  }, 600)
}

function handleInput(e) {
  setTimeout(() => {
    let data = e.data
    if (!data || isCompositing) {
      return
    }
    console.log(data.split(''), index.value, 'data.split(\'\')')
    str.value.splice(index.value + 1, 0, ...data.split(''))
    console.log(data.length, data, 'data.length')
    console.log()
    index.value = +index.value + data.length
    setTimeout(() => {
      setCursor(index.value)
    })

    console.log(index.value)
    inputDom.value.value = ''
  }, 0)

}

function handleKeydown(e) {
  if (e.keyCode === 8) {
    // delete()

    if (!(index.value > -1)) {
      return
    }
    str.value.splice(index.value, 1)
    index.value = index.value - 1
    setCursor(index.value)
  } else if (e.keyCode === 13) {
    // this.newLine()
  }
}

function hanldePaste(e) {
  e.preventDefault()
  e.data = e.clipboardData.getData('text')
  handleInput(e)
}

function compositionstart() {
  isCompositing = true // 是否正在输入拼音
  console.log('compositionstart')
}

function compositionend() {
  isCompositing = false
}

onMounted(() => {
  // document.addEventListener('click', function(event) {
  //   var target = event.target;
  //   console.log(target.nodeType,Node.TEXT_NODE,'target.nodeType')
  //   if (target.nodeType === Node.TEXT_NODE) {
  //     var range = document.createRange();
  //     range.selectNodeContents(target);
  //     var rects = range.getClientRects();
  //     for (var i = 0; i < rects.length; i++) {
  //       var rect = rects[i];
  //       if (event.clientX >= rect.left && event.clientX <= rect.right &&
  //         event.clientY >= rect.top && event.clientY <= rect.bottom) {
  //         var charIndex = range.getCharIndexAtPoint(event.clientX, event.clientY);
  //         console.log('用户点击了文本：' + target.textContent.charAt(charIndex));
  //         break;
  //       }
  //     }
  //   }
  // });
})
</script>
<style>
.about {
  position: relative;
}
</style>
