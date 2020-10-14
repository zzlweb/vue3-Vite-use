<template>
  <div>
    <h1>当前鼠标的位置</h1>
    <div>{{ msg }}</div>
    <div>{{ x }}, {{ y }}</div>
  </div>
</template>

<script>
import { reactive, onMounted, onUnmounted, toRefs, ref } from 'vue'

function mouseHandle() {
  const mouse = reactive({
    x: 0,
    y: 0
  })
  const move = (e) => {
    mouse.x = e.pageX
    mouse.y = e.pageY
  }
  onMounted(() => {
    document.addEventListener('mousemove', move)
  })

  onUnmounted(() => {
    document.removeEventListener('mousemove', move)
  })
  return mouse
}

export default {
  setup() {
    const mouse = mouseHandle()
    const msg = ref('hello')
    return {
      ...toRefs(mouse),
      msg
    }
  }
}
</script>
