<template>
  <div id="app"></div>
</template>

<script>
export default {
    setup(){
      console.log('setup执行了')
      // setup 没有 this 
      console.log(this)
    }, 
    
    // setup 在beforeCreate 之前执行
    beforeCreate(){
      console.log('beforecreate');
    }
}
</script>

<style>

</style>