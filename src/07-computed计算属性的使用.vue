<template>
  <div class="app">
    <div>我今年的年龄--- <input type="text" v-model="age">
    </div>
    <div>我明年的年龄： <input type="text" v-model="nextAge"></div>
    <div class="">我上一年的年龄： <input type="text" v-model="prevAge"></div>
  </div>
</template>

<script>
import { ref , computed} from 'vue'
export default {
  setup(){
    const age = ref(18)

    // computed是一个函数
    // 第一种写法： 传入一个函数相当于getter 返回一个计算属性

    const nextAge = computed(() => {
      return parseInt(age.value) + 1 
    })

    // 第二种写法： 传入一个对象， 包括get 函数和 set 函数

    const prevAge = computed({
      get(){
        return parseInt(age.value) - 1 
      },

      set(value){
        age.value = value - 2 
      }
    })

    return {
      age , 
      nextAge, 
      prevAge
    }
  }
}
</script>

<style>

</style>