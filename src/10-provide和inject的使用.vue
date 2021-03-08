<template>
  <div class="app">
    <Demo v-bind='money'></Demo>
  </div>
</template>

<script>
import Demo from './11-inject接受数据.vue'
import { ref , provide , reactive} from 'vue'
export default {
  components:{
    Demo
  }, 
  setup(){
    const money = ref(100)
    const changeMoney = (value )=>{
      money.value = value 
    }
    // 父组件提供money
    provide('money', money)
    provide('changeMoney', changeMoney)
    return { money }
  }
}
</script>

<style>

</style>