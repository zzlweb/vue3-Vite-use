<template>
  <div class="app" ref="appRef"></div>
</template>

<script>
// 在vue3.0中使用ref 操纵DOM

import { onMounted, ref } from 'vue'
export default {
  setup(){
    // 首先声明ref
    const appRef = ref(null)
  },

  // 使用ref
  onMounted(){
    console.log(appRef.value);
  }
}
</script>

<style>

</style>