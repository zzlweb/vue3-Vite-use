<template>
  
</template>

<script>
import { ref , reactive, toRefs, watch } from 'vue'
export default {
  setup(){
    const state = reactive({
      money: 100 , 
      car: {
        brand:'宝马'
      }
    })

    // 接收3个参数
    // 参数1：要监听的数据源 可以是一个ref 或者是一个函数
    // 参数2：回调函数 （value , oldValue）=>{}
    // 参数3：额外的配置 是一个对象 { deep: true , immediate: true }
    watch( () => state.money , (value, oldValue)=>{})

    watch(money , (value , oldValue)=> {})

    watch(() => state.car , (value)=> {}, {deep: true , immediate: true })
  }
}
</script>

<style>

</style>