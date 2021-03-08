# vue3-Vite-use
Vue3.0常用属性写法。

#### $attrs的使用
包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定属性 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。

#### 父组件
```vue
<template>
  <div class="father">
    <Son msg1="123" msg2="456"></Son>
  </div>
</template>

<script>
import Son from "./son.vue";
export default {
  components:{Son},
};
</script>
```

#### 子组件

```vue
<template>
    <div class="item" v-bind="$attrs">123</div>
    <div class="item" :msg1="$attrs.msg1">123</div>
    <div class="item" :msg2="$attrs.msg2">134</div>
</template>

<script>
export default {
  // 当子组件不使用props接受来自父组件的值时，属性值（非动态值属性不被props识别）会被继承到子组件身上，使用inheritAtters: false 设置不继承。
  // inheritAtters: false 
  mounted(){
    // 使用$attrs使用传递的属性值
    console.log(this.$attrs.msg1);
  }
}
</script>
```