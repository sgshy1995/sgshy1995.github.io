# Vue 解析：.sync 修饰符


## 用途

#### 特别的需求：

- 当一个子组件改变了一个外部传入的属性时，这个变化也会同步到父组件中所绑定。
- 子组件要“通知”父组件，才可以改变。
- 避免真正的“双向绑定”，让子组件改变父组件传入的属性或状态的这种改动来源更容易被区分。

## Vue 的推荐

Vue 推荐以 `update:myPropName` 的模式触发事件取而代之，为了方便起见这种模式提供一个缩写，即[.sync修饰符](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6)。

## 应用举例

### 基本项目

父组件 app 传入一个值给子组件 child，子组件更改这个值并渲染，且父组件同步渲染更新。

*index.html*

```html
<body>
  <div id="app"></div>
</body>
```

*main.js*

```js
// vue.runtime
import Vue from 'vue'
import App from './app.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

*app.vue*

```vue
<template>
  <div id="app">
    {{totalNumber}}
    <Child :childNumber.sync="totalNumber" />
  </div>
</template>

<script>
import Child from './components/child.vue'
export default {
  data(){
    return{
      totalNumber: 10000
    }
  },
  components:{
    Child: Child
  }
}
</script>
```

*child.vue*

```vue
<template>
    <div>
        {{childNumber}}}
        <button @click="$emit('update:childNumber', childNumber-100)">减100</button>
    </div>
</template>

<script>
export default {
    props:['childNumber']
}
</script>
```

在点击“减100”按钮时，父组件和子组件的数值展示会同时更新渲染。

### “儿子向父亲要钱” 的项目

一个用 vue 的 .sync 修饰符实现的[项目](#)，同时包含了其他的一些修饰符的示意。

![示意图](/images/vue/1.gif)

## vue 的规则

- 组件不能修改 props 外部数据；
- $emit 可以触发事件，并传参；
- $event 可以获取到 $emit 传入的参数。
- 这也是这个功能实现的原理。

## 修饰符扩展

`:childNumber.sync="totalNumber"`

等价于

`:childNumber="totalNumber" v-on:update:childNumber="total=$event"`