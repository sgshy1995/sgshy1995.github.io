# Vue使用实例：vue/vue.runtime


## 前言

本文假设：
- 你已经使用 [vue-cli](https://cli.vuejs.org/zh/guide/) 生成了一个 vue 项目。或参照 [小教程](http://eden-sheng.cn/2019/12/vuecli-one/)。

- 并且，src 目录中只有 main.js，public 目录中只有 index.html。

*index.html*

```html
<body>
    <div id="app"></div>
</body>
```

## 创建实例

通过用 Vue 函数创建一个新的 Vue 实例开始：

```js
import Vue from 'vue'

var vm = new Vue({
  // options
})
```

Vue 类似于 [MVVM模型](https://zh.wikipedia.org/wiki/MVVM)，经常会使用 **vm (ViewModel 的缩写)** 来表示 **Vue 实例**。

vm 封装了这个实例对于组件内容的所有操作，包括数据、事件绑定等。

### 初始化

我们使用 MVC 来封装这个 div，并使用模板语法的一种占位符来将变量 n 变为字符串插入 html。

*index.html*

```html
<body>
  <div id="app">{{n}}</div>
</body>
```

*main.js*

```js
import Vue from 'vue'

new Vue({
    // 元素 element
    el: '#app'
})
```

### 运行服务

创建一个本地服务。

```bash
$ yarn serve
```

### Vue 不支持你这样写？

这时浏览本地页面，看不到任何渲染效果，并且控制台会警告：

```bash
# You are using the runtime-only build of Vue where the template compiler is not available. 
# Either pre-compile the templates into render functions, or use the compiler-included build.

# (found in <Root>)
```

简单的意思来说，就是你在使用 Vue 的 runtime-only 版本，Vue 不支持你这样写。

## Vue 的两个版本

Vue 有两个版本。

- 完整版
  * vue.js
  * vue.min.js
- 只包含运行时版（非完整版）
  * vue.runtime.js
  * vue.runtime.min.js

vue-cli 创建的 vue 项目，默认的配置是 vue.runtime.js 版本。

下面通过实际在项目中的操作来说明两者的区别。

### 完整版

完整版，即 vue.js。

我们通过 CDN 来引入完整版取代 import 方法。

*index.html*

```html
<body>
  <div id="app">{{n}}</div>
  <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</body>
```

*main.js*

```js
new Vue({
  // 元素 element
  el: '#app',
  data: {
    n: 0
  }
})
```

可以看到，在 MVC 模型中，视图 el 并没有写在 js 文件中，而是直接写在 html中。

当把 el 写入 js 中：

*index.html*

```html
<body>
  <div id="app"></div>
  <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</body>
```

*main.js*

```js
new Vue({
  // 元素 element
  el: '#app',
  template: `
    <div>{{n}}</div>
  `,
  data: {
    n: 0
  }
})
```

结果是一样的。仍然支持。

### 只包含运行时版（非完整版）

对于只包含运行时版，以下简称为非完整版。

当你引入非完整版时，使用以上方法，页面中再次消失渲染效果。说明这个Vue版本不支持你这样编写。

### 两者的区别

如果你需要在客户端编译模板 (比如传入一个字符串给 template 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板)，
 只包含运行时版（非完整版）不支持，而 完整版 支持。

**简单来说就是：**

只要从html渲染为页面中的结果，那么只 包含运行时版（非完整版）不支持，而 完整版 支持。

## 非完整版的存在意义

仍然通过CDN引入非完整版的Vue。

*index.html*

```html
<body>
  <div id="app"></div>
  <script src="https://cdn.bootcss.com/vue/2.6.10/vue.runtime.min.js"></script>
</body>
```

### 简单插入

如果你想实现上面的效果，那么你需要：

使用一个 render 函数，他接受一个 h 参数，这个参数由 Vue 传入。

*main.js*

```js
new Vue({
  // 元素 element
  el: '#app',
  render(h){
      // 创建一个div 里面是 n
    return h('div', this.n)
  },
  data: {
    n: 0
  }
})
```

### 加入事件监听

如果我们在一个按钮的点击事件中，将 n 加一。

*main.js*

```js
new Vue({
  // 元素 element
  el: '#app',
  render(h) {
      // 创建一个 div，div 里面是 n 和 button  
      // button 的事件监听是 点击后触发 add方法，button 里面(的内容)是 Click me to add one
    return h('div', [this.n,h('button', {on:{click:this.add}}, 'Click me to add one')])
  },
  data: {
    n: 0
  },
  methods: {
      // add 方法
    add() {
      this.n += 1
    }
  }
})
```

### “表面上”的弊端

通过这种方法，有一个明显的“表面上”的弊端：

你必须要用 render 方法并传入 h 参数把所有的元素构造出来。

### 意义

那么它的意义在哪里？

最大的意义就在于 **体积小**。

完整版含有编译器（compiner），将含有占位符或条件循环的语句等变为真实的DOM节点。在你下次修改时，会去直接改DOM节点，来生成 HTML。但是它非常的复杂，也就是代码体积会很大。

**非完整版要比完整版小 30% 左右。**

## 两全其美：单文件组件

我们有一种两全其美的方法：

使用 webpack 的 vue-loader。（注意：我们在使用 vue-cli 时，它已经帮我们配置好了这个加载器。）

我们在写组件的时候，在 build 时这个加载器会自动帮我们编译为 render 函数的方法 来构造。

这样，用户下载产品时只需要下载体积小的非完整版，而开发人员编写代码时，也不需要使用 render 方法去构造元素。

我们来这样再次实现上面的效果。

### 创建 vue 文件

默认配置是非完整版的 vue，其实我们无需再外部引入。

```html
<body>
  <div id="app"></div>
</body>
```

首先，需要创建一个vue文件。

*Demo.vue*

```vue
<template>
  <div class="demo">
    {{n}}
    <button @click="add">Click me to add one</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      n: 0
    };
  },
  methods: {
    add() {
      this.n += 1;
    }
  }
};
</script>

<style scoped>
.demo{
    color:blue;
}
</style>
```

在 template 中写 视图相关，在 script 中写 视图之外的所有内容，在 style 中写 CSS相关。

**vue-loader 就可以把这些变为一个 对象。**

### js中引入

在 js 中这样写：

*main.js*

```js
import Vue from 'vue'
import Demo from './Demo.vue'

new Vue({
  render(h) {
    return h(Demo)
  }
}).$mount('#app')
```

这样 class为"demo" 的 div 就替换掉了 id为"app"的 div。并且 div 里有 n 和 button。

**这也叫做 Vue 单文件组件。**

## 写在最后

Vue-cli 生成的项目中，默认配置就是 vue 的只包含运行时版（非完整版）；
你只需使用单文件组件就可以快速、方便的使用Vue实例。

Webpack 给前端带来了无限可能。
