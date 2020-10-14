<template>
  <div class="app">
    <div>{{ car.brand }} -- {{ car.price }}</div>
    <br>
    <button @click="car.brand = '奔驰'">修改</button>
  </div>
</template>

<script>
import { reactive} from 'vue'
export default {
  setup(){
    // setup 需要有返回值才能在模板中使用
    // reactive 中传入一个普通的对象，返回一个响应式对象

    const car = reactive( {
      brand: '宝马', 
      price: 100
    })

    return { 
      car 
    }
  }
}
</script>

<style>
</style>