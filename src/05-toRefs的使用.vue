<template>
  <div class="app">
    <div>{{money}}</div>
    <div>{{ car.brand }} --- {{ car.price}}</div>
    <div>{{ name }}</div>
    <button @click="money++">修改</button>
    <button @click="name='ls'">修改</button>
  </div>
</template>

<script>
import { ref , toRefs , reactive } from 'vue'
export default {
  setup(){
    const state = reactive({
      money: 1000, 
      car: {
        brand: '宝马', 
        price: 1000
      }, 
      name: 'zs'
    })

    return {
      ...toRefs(state)
    }
  }
}
</script>

<style>

</style>