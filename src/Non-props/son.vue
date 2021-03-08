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
