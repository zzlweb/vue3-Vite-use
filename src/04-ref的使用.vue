<template>
  <div class="app">
    <div>我的金钱： {{ money.value }} {{ car.price }}</div>
    <button @click="money++">修改</button>
  </div>
</template>

<script>
import { reactive , ref} from 'vue'
export default {
  setup(){
    let money = ref(100)
    let car = reactive({
      price: 1000
    })
    return { money, car  }

  }
}
</script>

<style>

</style>