<template>
  <div>{{ money }}</div>
  <div>{{car.brand}} --- {{ car.price }}</div>
</template>

<script>
import { reactive , ref, toRefs, readonly } from 'vue'
export default {
  setup(){
    const money = ref(100)

    const car = readonly({
      brand : 'zs', 
      price: 17
    })

    return {
      money: readonly(money), 
      car 
    }
  }
}
</script>

<style>

</style>