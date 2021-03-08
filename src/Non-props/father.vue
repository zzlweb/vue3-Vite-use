<template>
  <div class="father">
    <Son msg1="123" msg2="456"></Son>
  </div>
</template>

<script>
// $attr的使用
import Son from "./son.vue";
export default {
  components:{Son},
};
</script>