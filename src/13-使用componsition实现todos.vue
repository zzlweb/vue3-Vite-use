<template>
  <div>
    <input type="text" :value="inputValue" @change="inputChange" />
    <button @click="handleSubmit(inputValue)">提交</button>

    <ul>
      <li v-for="(item, index) in todos" :key="index">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
import { ref, reactive } from "vue";

// 关于操作todos 
const todosEffect = () =>{
    const todos = reactive([])
    const handleSubmit = (item) => {
      todos.push(item)
    }
    return {
      todos,handleSubmit
    }
  }

// 关于操作inputValues
const inputValueEffect = () => {
  const inputValue = ref("");
  const inputChange = (e) => {
      inputValue.value = e.target.value
    }
  return {
      inputChange, inputValue
    }
}
export default {
  setup() {
    const {todos,handleSubmit} = todosEffect()
    const {inputChange, inputValue} = inputValueEffect()

    return {
      todos,handleSubmit,
      inputChange, inputValue
    };
  },
};
</script>
