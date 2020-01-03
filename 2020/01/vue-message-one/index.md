# Vue组件通信的一种实现+坑


## 组件通信

前端在用 Vue 框架进行开发时，使用核心思想之一的组件式开发。

在源生JS中，我们熟悉数据之间的通信，页面之间的通信；在这里，Vue 通过组件通信来实现所有互动。

这里介绍父子组件、兄弟组件的一种常用而便捷的通信方式，同时在其中不经意间也有一些小小的坑。

## 父子组件

### 父子组件之 props

Prop 的[单向数据流](https://cn.vuejs.org/v2/guide/components-props.html#%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E6%B5%81)。

### 父组件给子组件传值

*父组件 App.vue*

```js
<template>
  <div id="app">
    {{father}}
    <Childone :child="father" />
  </div>
</template>

<script>
import Childone from "./components/childone"
export default {
  name: 'app',
  data(){
    return{
      father:100
    }
  },
  components:{
    Childone
  }
}
</script>
```

*子组件 childone.vue*

```js
<template>
  <div class="one">
      {{child}}
  </div>
</template>

<script>
export default {
    props:['child'],
}
</script>
```

在父组件的 template 中对子组件的引用，通过 `:child="father"` 来将 father 的值赋予 子组件中的 child。

### 子组件申请改变 props

在 Vue 文档中：

*额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。*

使用 `$emit` 和 `$event` 来实现子组件向父组件“申请”修改 props 并同步更新父组件的需求。

- 子组件通过在一个事件(比如 click 等)中使用 `$emit` 来触发另外一个事件并传出一个参数
- 父组件定义对该事件的监听，来响应子组件的变化
- 父组件可以通过 `$emit` 来获取子组件传出的参数

*子组件 childone.vue*

```js
<template>
  <div class="one">
      {{child}}
      <button @click="$emit('update:child',child-100)">减100</button>
  </div>
</template>

<script>
export default {
    props:['child'],
}
</script>
```

触发的这个事件名叫做 `update:child`，传出的参数为 `child-100`。

父组件对这个事件进行监听，并且做出响应 `father=$event`，可以说 `$event === child-100`。

*父组件 App.vue*

```js
<template>
  <div id="app">
    {{father}}
    <Childone :child="father" v-on:update:child="father=$event" />
  </div>
</template>

<script>
...
</script>
```

父子组件之间的通信实现。

## 兄弟组件

使用 Vue 中的 `中央事件总线(eventBus)` ， 用 `eventBus.$emit` 来触发事件， 用 `eventBus.$on` 来监听事件。

其实这种方式可以用于任何的父子组件通信以及同级别组件(兄弟组件)的通信。

*main.js*

```js
let eventBus = new Vue()
Vue.prototype.$eventBus = eventBus

new Vue(...).$mount(...)
```

*子组件1 childone.vue*

```js
<template>
  <div class="one">
      {{word}}
      <button @click="putWord">告诉兄弟</button>
  </div>
</template>

<script>
export default {
    data(){
        return{
            word:'Hello'
        }
    },
    methods:{
        putWord(){
            this.$eventHub.$emit('put',this.word)
        }
    }
}
</script>
```

*子组件2 childtwo.vue*

```js
<template>
    <div class="two">
      {{wordNew}}
    </div>
</template>

<script>
export default {
  data() {
    return {
      wordNew: ''
    };
  },
  created(){
    this.$eventHub.$on("put", dataFrom => {
      this.wordNew = dataFrom;
    });
  }
};
</script>
```

在子组件1中点击 button 即可将数据传给子组件2。

## 一个坑

**注意：定义 eventBus 的位置，一定要在创建 Vue 实例之前，否则 子组件2在创建和挂载之前的原型中将不会有 eventBus 存在。但是子组件1的功能却正常，因为在你触发点击事件时，已经使用新的原型。**

**子组件2的 eventBus.$on 将会变为 undefined。**

当你在创建一个 Vue实例 之后才定义 eventBus。

*main.js*

```js
new Vue(...).$mount(...)

let eventBus = new Vue()
Vue.prototype.$eventBus = eventBus
```

### 解决一

子组件2可以通过 watch 来触发使用新的原型。

*子组件2*

```js
<script>
export default {
  data() {
    return {
      wordNew: ''
    };
  },
  watch: {
    wordNew: {
      handler() {
        this.$eventHub.$on("put", dataFrom => {
          this.wordNew = dataFrom;
        });
      },
      deep: true,
      immediate: true
    }
  }
};
</script>
```

### 解决二

或强行对 子组件2 产生一个更新，在 updated 中再使用 eventBus。

*子组件2*

```js
<script>
export default {
  data() {
    return {
      wordNew: 'wordNew'
    };
  },
  created() {
    setTimeout(() => {
      this.wordNew = ''
    }, 0);
  },
  updated() {
    this.$eventHub.$on("put", dataFrom => {
      this.wordNew = dataFrom;
    });
  }
};
</script>
```

这个坑也是我自己在实现用 eventBus 实现组件通信时发现的，并且用两种不同的尝试“解决”了这个有趣的坑。

：）