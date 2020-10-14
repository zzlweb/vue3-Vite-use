<template>
  <div class="demo"> demo ---{{ money }}</div>

  <button @click="fn">修改</button>
</template>

<script>
import { inject } from 'vue'
export default {
  setup(props){

    console.log(props);
    const money = inject('money')
    
    const changeMoney = inject('changeMoney')
    const fn = () => {
      changeMoney(200)
    }
    return {
      money, 
      fn
    }
  },
  props:{
    money: Number
  }
}
</script>

<style>

</style>