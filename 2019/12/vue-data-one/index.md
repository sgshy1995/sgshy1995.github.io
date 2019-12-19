# new Vue( )：数据响应式


## 前言

Vue 到底对 data 做了什么呢？我们来看下以下代码。

为了方便操作，这里引入完整版 Vue。

*index.html*

```html
<body>
  <div id="app"></div>
  <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</body>
```

*main.js*

```js
const Vue = window.Vue

const myData = {
  n:0
}

new Vue({
  data: myData,
  template: `
    <div>{{n}}</div>
  `
}).$mount('#app')

setTimeout(() => {
  myData.n += 10
}, 0)

console.log(myData)
```

我们可以看到，在将 myData 传给 Vue 时，会被篡改。

控制台：

```t
# {__ob__: we}
#   n: (...)
#   __ob__: we {value: {...}, dep: ce, vmCount: 1}
#   get n: f ()
#   set n: f (t)
#   __proto__: Object
```

## ES6 的 getter/setter

### 打印名字

下面是使用 ES6 新语法的例子：

```js
let obj = {
  firstName: 'Eden',
  lastName: 'Sheng',
  name() {
    return this.firstName + this.lastName
  }
}

console.log(obj.name()) 

//EdenSheng
```

控制台可以打印出 EdenSheng 。

### 使用计算属性

如果我们使用 get 方法：

```js
let obj = {
  firstName: 'Eden',
  lastName: 'Sheng',
  get name() {
    return this.firstName + this.lastName
  }
}

console.log(obj.name)

//EdenSheng
```

这时候可以通过使用 get 来直接使用 obj.name 这个计算属性，同样可以打印出 EdenSheng。

这种写法就叫做 getter ，关键词为 get ，用于获取一个值。**定义时为函数，但是使用时不用加括号。**

### 修改并打印

通过 set 来设置姓名：

```js
let obj = {
  firstName: 'Eden',
  lastName: 'Sheng',
  get name() {
    return this.firstName + this.lastName
  },
  set name(e) {
    this.firstName = e[0]
    this.lastName = e.slice(1)
  }
}

obj.name = 'xy'

console.log(obj.name, `${obj.firstName},${obj.lastName}`) 

//xy x,y
```

在 `obj.name = 'xy'` 时就相当于触发 set 函数。 setter 用于对数据的改写。

### 观察这个对象

我们来看一下这个 obj 是什么。

```js
...
console.log(obj)
```

```t
# {firstName: "x", lastName: "y"}
#   name: (...)
#   firstName: "x"
#   lastName: "y"
#   get name: f name()
#   set name: f name(e)
#   __proto__: Object
```

### 结论

经过 vue 包装，以及我们这样写的 obj，n 和 name 都不是真实的属性；
浏览器在显示的时候是 (...)，表示可以对n 和 name 进行读和写，但是他们并不真实存在，通过 get 和 name 来模拟对其的读写操作。

## 如何对已声明对象操作？

我们如何对于一个已经声明的对象，为其定义一个新的属性 ？

### Object.defineProperty

使用 [Object.defineProperty( )](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 方法。

Object.defineProperty( ) 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

#### 语法

`Object.defineProperty(obj, prop, descriptor)`

#### 参数
- obj 
  - 要在其上定义属性的对象。
- prop 
  - 要定义或修改的属性的名称。
- descriptor 
  - 将被定义或修改的属性描述符。

#### 返回值

 被传递给函数的对象。

### 例子

为已声明对象 data 定义新的属性 n。

```js
let data = {}

Object.defineProperty(data, 'n', {
  value: 0
})

console.log(data)

//打印结果为 {n: 0}
```

### 问题

当我们有这样一个需求： n不能小于0。但是 n 其实是并不存在的，我们如何对其进行判断？

在 data 中“悄悄”搞一个变量来存储 n 。

```js
let data = {}

data._n = 0

Object.defineProperty(data, 'n', {
  get() {
    return this._n
  },
  set(value) {
    if (value < 0) return
    this._n = value
  }
})

console.log(data.n)
// 0

data.n = -1
console.log(data.n)
// 0

data.n = 1
console.log(data.n)
// 1
```

**那么这样就有一个问题：** 如果直接从外部访问并篡改 data._n ，你的判断逻辑部分并不能察觉到这一点，就会造成混乱。

如何解决这个问题？ 使用代理和监听。

## 代理/监听

### 使用代理

代理也是一种设计模式，使用代理来实现上述需求。构造一个 proxy 函数。

```js
function proxy({ data }) {
  const obj = {}
  Object.defineProperty(obj, 'n', {
    get() {
      return data.n
    },
    set(value) {
      if (value < 0) return
      data.n = value
    }
  })
  return obj
}

let dataProxy = proxy({ data: { n: 0 } })

console.log(dataProxy.n)
// 0

dataProxy.n = -1
console.log(dataProxy.n)
// 0

dataProxy.n = 1
console.log(dataProxy.n)
// 1

```

函数中首先声明了一个空对象 obj。当你读写 dataProxy 时，实际得到的返回值为 obj，而 obj 其实就是 data 的返回结果。

这样，在他们之间，就实现了一层代理： obj。我们暴露给用户或外面的只有一个对象：obj。

### 使用监听

#### 当 绕过代理

如果你在初始化时声明一个引用，可以绕过代理直接读写。

```js
let myData = {n: 0}
let dataProxy = proxy({data: myData})
...
console.log(dataProxy.n)
// 0

myData.n = -1
console.log(dataProxy.n)
// -1
```

#### 添加监听

为对象添加监听：

```js
function proxy({ data }) {

  // 将 data.n 储存在 value 中
  let value = data.n
  //这句可写可不写，接下来的定义新属性时如果发现n已经存在会自动覆盖
  delete data.n
  Object.defineProperty(data, 'n', {
    get() {
      return value
    },
    set(newValue) {
      if (newValue < 0) return
      value = newValue
    }
  })

  /* 以上即添加监听 */

  /* 以下对新的 "data" 进行代理 */

  const obj = {}
  Object.defineProperty(obj, 'n', {
    get() {
      return data.n
    },
    set(value) {
      if (value < 0) return
      data.n = value
    }
  })
  return obj
}

let myData = {n: 0}
let dataProxy = proxy({data: myData})

console.log(dataProxy.n)
// 0

myData.n = -1
console.log(dataProxy.n)
// 0

myData.n = 1
console.log(dataProxy.n)
// 1
```

**注意**：我们只是在修改对象 {n: 0}，监听就是修改对象的过程，代理是在被修改过的原始对象上创建的。这样，彼此的连接才不会断开。

### 多个属性

当你有的 data 中有多个变量/属性时，可以用 闭包 和 循环 来实现这个过程。

## new Vue()

### vue 对 data 做了什么？

当你创建一个实例时

`const vm = new Vue({data: myData})`

- vue 会让 vm 成为 myData 的代理。
- vue 会对 myData 的所有属性进行监控。

### 目的

这样做的目的是什么？

- 你可以使用 this 来访问到 vm。 this.n === myData.n。
- 之所以要监控，就是防止 vue 无法得知 myData 的属性变化。
- vue 得知属性变化才可以使用 render(data) 来更新 UI 和渲染页面。

## 数据响应式

- 响应式即对外界的变化做出的反应的一种形式。
- const vm = new Vue({data:{n: 0}})
- 当修改 vm.n 或 data.n 时，render(data...) 中的 n 就会做出响应的响应。
- **这个联动的过程就是 vue 的 数据响应式。**
- vue 目前通过 Object.defineProperty 来实现数据响应式。

## 在 data 中添加属性

Vue 虽然对 data 中的属性（或对象中的属性）进行监听和代理，但是它却没有办法进行事先的监听和代理。

如果你在初始化 data 之后再添加属性，该如何实现？

### 一般对象

对于一般的对象来说，可以在 data 中预先把所有可能用到的属性全部写出来，这样并不需要新增属性，只需要改它。

也可以通过其他方法来添加属性。

在了解以上原理后，我们来了解 Vue 提供的一个 API：

`Vue.set(object, key, value)`

或

`this.$set(object, key, value)`

#### 作用

- 在 data 中添加新的属性。
- 自动创建为它创建代理和监听（如果没有创建过）。

#### 示例

```js
const Vue = window.Vue

new Vue({
    data: {
        obj: {
            a: 0
        }
    },
    template: `
    <div>
      {{obj.b}}
      <button @click='one'>One</button>
    </div>
    `,
    methods: {
        one() {
            Vue.set(this.obj, 'b', 1)
            // 或 this.$set(this.obj, 'b', 1)
        }
    }
}).$mount('#app')
```

### 数组

因为数组本身的特殊性：数组的长度无法预测（比如所有用户的用户名，存在数组中），你无法使用 undefined 去为每一项占位，或一直使用 Vue.set( ) 方法。

- 你可以使用 push 方法 `this.array.push('value')`，但其实数组已经被 Vue 包装了新的 push 方法。
- 原理就是声明一个新的类来继承数组。
- 各种在 Vue 实例 中使用的特例方法， 详见[数组变异方法](https://cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E5%BC%82%E6%96%B9%E6%B3%95-mutation-method)，一共有7个API。
- 这些方法 (API) 会自动处理对数组该项的监听和代理，并触发视图更新。

## 什么才是最重要的

- 研究思路和方法永远比知识更重要。

- 你可以在不懂、不看源代码的前提下，去使用和理解一个库或项目或其他。

- 前提是你已经知道了它的思路和方法。

