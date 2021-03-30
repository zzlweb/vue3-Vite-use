# vue3-use
Vue3.0常用属性写法。

###  [一个小的商城案例](https://github.com/zzlweb/vue3-small-mall)

### setup 函数的使用

```vue
<template>
  <div>
    {{ item }}
  </div>
</template>

<script>
export default {
  // created实例被完全初始化之前执行,代替beforeCreate, created
  setup(props, context) {
    // setup接受两个参数, 参数一: props 同 vue2
    // 参数二: context 为一个对象 { attrs, slots, emit}
    console.log(props, context);

    return {}
  },
};
</script>
```

### ref, reactive响应式引用的使用
```vue
<template>
  <div>
    {{ item }}
  </div>
</template>

<script>
//  ref, reactive 响应式引用
//  原理，通过 proxy 对数据进行封装，当数据变化时，触发模版等内容的更新
// ref 处理基础类型的数据
// reactive处理复杂类型的数据
import { ref, reactive, toRefs } from "vue";
export default {
  setup() {
    const item = ref(0);
    const MyObj = reactive({
      name: "zs",
      age: 14,
    });
    // 直接结构myObj得到name并不具有响应式。
    // const { name} = MyObj
    // return {item ,name}

    // 例如
    // reactive({ name: 'dell'}) 变成 proxy({ name: 'dell'}) 这样的一个响应式引用, name 并没有响应式。

    // toRefs proxy({ name: 'dell', age: 28}), {
    //  name: proxy({ value: 'dell'}),
    //  age: proxy({value: 28})
    // }
    
    return {
      item,
      ...toRefs(MyObj),
    };
  },
};
</script>
```

### torefs 和 toRef 的使用
```vue  
<template>
  <div>{{ name }}</div>
</template>

<script>
// toRefs 和 toRef 的使用
import { reactive, toRefs } from "vue";
export default {
  setup() {
    const myObj = reactive({
      name: "zs",
      age: 123,
    });

    return {
      ...toRefs(myObj),
    };
  },
};
</script>
```

```vue
import { reactive , toRef } from 'vue'
<script>
// 可以用来为源响应式对象上的 property 性创建一个 ref。然后可以将 ref 传递出去，从而保持对其源 property 的响应式连接。
export default {
  setup() {
    const state = reactive({
      foo: 1,
      bar: 2,
    });

    const fooRef = toRef(state, "foo");

    fooRef.value++;
    console.log(state.foo); // 2

    state.foo++;
    console.log(fooRef.value); // 3
  },
};
</script>

```


### 使用 ref 操作dom的写法(3.0)。
声明ref对象，在dom上使用ref属性进行关联。通过 ref.value 进行访问。

```vue
<template>
  <div class="app" ref="appRef"></div>
</template>

<script>
// 在vue3.0中使用ref 操纵DOM
import { onMounted, ref } from 'vue'
export default {
  setup(){
    // 首先声明ref
    const appRef = ref(null)
  },
  // 使用ref
  onMounted(){
    console.log(appRef.value);
  }
}
</script>
```

### 父子组件通讯(3.0)
描述父子组件之间如何通过事件进行通讯

####  父组件
```vue
<template>
  <div>
      <son v-model="count"></son>
  </div>
</template>

<script>
import { reactive,toRefs } from 'vue'
import Son from "../components/son";
export default {
  components:{Son},
  setup(){
    const data = reactive({
      count : 123
    })
    
    return{
      ...toRefs(data)
    }
  }
};
</script>
```

####  子组件
```vue
<template>
  <div @click="handleClick">{{ modelValue }}</div>
</template>

<script>
// import { toRefs } from "vue";
export default {
  props:['modelValue'],
  setup(props, { emit }) {
    const handleClick = () => {
      emit("update:modelValue", props.modelValue + 3);
    };

    return { handleClick};
  },
};
</script>
```
####  自定义绑定值

父组件

```vue
<template>
  <div>
      <son v-model:inputVal="data"></son>
  </div>
</template>

<script>
import { ref } from 'vue'
import Son from "../components/son";
export default {
  components:{Son},
  setup(){
    const data = ref(0)

    return{
      data
    }
  }
};
</script>
```
子组件

```vue
<template>
  <input type="text" :value="inputVal" @change="handleClick" />
</template>

<script>
// import { toRefs } from "vue";
export default {
  props: ["inputVal"],
  setup(props, { emit }) {
    const handleClick = () => {
      emit("update:inputVal", props.inputVal);
    };

    return { handleClick };
  },
};
</script>
```


### teleport传送门的使用

```vue
<template>
  <div class="area">
        <button @click="handleBtnClick">按钮</button>
        <teleport to="body">
          <div class="mask" v-show="show">{{message}}</div>
        </teleport>
      </div>
</template>

<script>
// teleport传送门。
export default {
  data() {
      return {
        show: false,
        message: '123'
      }
    },
    methods: {
      handleBtnClick() {
        this.show = !this.show;
      }
    },
}
</script>

<style  scoped>
.area {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      width: 200px;
      height: 300px;
      background: skyblue;
    }
    .mask {
      position: absolute;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      background: #000;
      opacity: 0.5;
      color: #fff;
      font-size: 10px;
    }
</style>
```

### h函数的使用
h函数返回虚拟的dom节点，接受三个参数，第一个参数： type：表示节点的类型，第二个参数props,表示节点的属性，第三个参数children表示节点的值。

dom的渲染过程: template --> render --> h --> vnode --> 真实的dom --> 显示到页面上

使用方式
```vue
父组件
<template>
  <div>
    <son :level="2">
      渲染内容
    </son>
  </div>
</template>

<script>
import son from "../components/son";
export default {
  components: {
    son,
  },
};
</script>

子组件
<script>
import { h } from  'vue'
//  h函数的使用
export default {
  props: ['level'],
    render() {
      return h('h' + this.level, {}, [
        this.$slots.default(), 
        h('h1', 'A headline'),
      ])
    }
};
</script>
```

### watch和watchEffect的使用
```vue
<template>
  <div>
    <div>Name: <input v-model="name" /></div>
    <div>Name is {{ name }}</div>
    <div>EnglishName: <input v-model="englishName" /></div>
    <div>EnglishName is {{ englishName }}</div>
  </div>
</template>

<script>
import { reactive, watch, watchEffect, toRefs } from "vue";
// watch监听器,watchEffect监听器,
export default {
  setup() {
    const nameObj = reactive({
      name: "dell",
      englishName: "lee",
    });
    // 具备一定的惰性 lazy
    // 参数可以拿到原始和当前值
    // 可以侦听多个数据的变化，用一个侦听器承载
    watch(
      [() => nameObj.name, () => nameObj.englishName],
      ([curName, curEng], [prevName, preEng]) => {
        console.log("watch", curName, prevName, "---", curEng, preEng);
      },
      { immediate: true }
    );

    // 立即执行，没有惰性 immediate
    // 不需要传递你要侦听的内容，自动会感知代码依赖，不需要传递很多参数，只要传递一个毁掉函数
    // 不能获取之前数据的值
    // const stop = watchEffect(() => {
    //   console.log(nameObj.name);
    //   console.log(nameObj.englishName);
    //   setTimeout(() => {
    //     stop();
    //   }, 5000)
    // })

    const { name, englishName } = toRefs(nameObj);

    return { name, englishName };
  },
};
</script>
```

### 混入mixins的使用（2.0）
```vue
<template>
  <div>
    {{data}}----{{number}}
  </div>
</template>

<script>
// mixin混入
// 组件data,methods优先级高于mixin data,methods 优先级。
// 声明周期函数，先执行mixin里面的，在执行组件里面的生命周期函数。
// 自定义属性，组件中的属性优先级高于mixin属性的优先级。

// 总结：mixin混入，除了生命周期混入高于组件，其他组件优先级高。
import { ref } from 'vue'
import mixins from './mixins/index'
export default {
  mixins:[mixins],
  setup(){
    const data = ref(0)
    return{
      data
    }
  }
};
</script>
```
mixins
```js
const myMixins = {
  data(){
    return {
      number : 1
    }
  }
}

export default myMixins
```

###  $attrs的使用(2.0)
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

### slot的使用（2.0）
####  默认插槽的使用

父组件
```vue
<template>
  <div>
    <son>123</son>
  </div>
</template>

<script>
import son from '../components/son'
export default {
  components:[son],
};
</script>
```
子组件
```vue
<template>
<div>
  <slot></slot>
</div>
</template>
```

####  具名插槽的使用
父组件
```vue  
<template>
  <div>
    <son>
      <template #one> 123 </template>

      <template #two> 456 </template>
    </son>
  </div>
</template>

<script>
import son from "../components/son";
export default {
  components: {son},
};
</script>

```
子组件
```vue
<template>
<div>
  <slot name="one"></slot>
  <slot name="two"></slot>
</div>
</template>
```

####  作用域插槽的使用
父组件

```vue  
<template>
  <div>
    <son v-slot="{item}">
      <div>{{item}}</div>
    </son>
  </div>
</template>

<script>
// 父组件通过v-slot接受数据,将结构传入进子组件。
import son from "../components/son";
export default {
  components: {son},
};
</script>

```

子组件

```vue
<template>
<div>
  <slot v-for="item in list" :item="item"></slot>
</div>
</template>

<script>
//  子组件提供数据，
import { reactive, toRefs } from 'vue'
export default {
  setup() {
    const list = reactive({
      list:[1,2,3]
    })

    return {
      ...toRefs(list)
    }
  },
}
</script>
```
