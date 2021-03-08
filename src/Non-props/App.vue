<template>
  <div class="father">
    <Son msg1="123" msg2="456"></Son>
  </div>
</template>

<script>
// $attr的使用
import { ref } from "vue";
import Son from "./son.vue";
export default {
  components:{Son},
  setup() {
    const msg1 = ref("hello");
    const msg2 = ref("world");
    return {
      msg1,
      msg2
    }
  },
};
</script>