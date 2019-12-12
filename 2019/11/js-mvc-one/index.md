# MVC：面包块而不是面条


## 概述
在软件工程中，[设计模式](https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F_(%E8%AE%A1%E7%AE%97%E6%9C%BA))（design pattern）是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。

设计模式并不直接用来完成代码的编写，而是描述在各种不同情况下，要怎么解决问题的一种方案。

## 模块化
面向对象设计模式 通常以类别或对象来描述其中的关系和相互作用。

设计模式能使不稳定依赖于相对稳定、具体依赖于相对抽象，避免会引起麻烦的紧耦合，以增强软件设计面对并适应变化的能力。

**简单来说，就是总有“万金油”的代码，是所有项目或块的通用代码；并且使用通用代码，可以使整体的结构更加清晰，彼此之间虽有联系，但是不会有那种“牵一发而“改”全身的情况。**

你的代码会 **Be Like Bread Nuggets** 而不是 **Just Like Noodles**。这就叫做代码“模块化”。

有些人把写的像 Shit 一样的代码称为 **Spaghetti Code**。

![noodle-code](/images/js-type/2.jpeg)


## MVC设计模式（思想）
MVC 是一种设计模式（或者软件架构），把系统分为三层：**Model数据**、**View视图**和**Controller控制器**。

实现到各个Javascript框架中都有多少的衍变，但都是从MVC用于Web设计开发衍生出去的。

举个例子：可能你在编写的时候觉得，一步接一步，逻辑已经很清晰了；在几天后，你回过头看自己的项目，你已经失去了头绪。
有很多重复的地方，改一步就要改所有，这其中就可能会有严重的错误产生。

而运用MVC思想，真正的把你的代码变成一个个分块，而不是一大坨堆在一起。

### Model（数据）
Model 数据管理，包括数据逻辑、数据请求、数据存储等功能。

前端 Model 主要负责 AJAX 请求或者 LocalStorage 存储。

```js
const model = {
    //定义数据
    data: null,
    //初始化
    init(){}
    //各种方法
    fetch(){}
    save(){}
    update(){}
    delete(){}
}
```

### View（视图）
View 负责用户界面，前端 View 主要负责 HTML 渲染。
```js
const view = {
    init() {}
    template: '<h1>hi</h1'>
}
```

### Controller（控制器）
Controller 负责处理 View 的事件，并更新 Model；也负责监听 Model 的变化，并更新 View，Controller 控制其他的所有流程。
```js
const controller = {
    view: null,
    model: null,
    init(view, model){
        this.view = view
        this.model = model
        this.bindEvents()
    }
    render(){
        this.view.querySelector('name').innerText = this.model.data.name
    },
    bindEvents(){}
}
```
### 为了“好代码”
MVC 只是一种设计模式，给了我们一种设计代码结构的主要思路。在设计模式实现的过程中，如果结合更多的方法与理念，可以让代码向真正的“好代码”靠近。

**主要的结构是几个面包块，面包块里也有面包块。**

使用 “表驱动编程”思想 和 EventBus方法。

## 表驱动法编程
### 什么是表驱动法
所谓[表驱动法](https://baike.baidu.com/item/%E8%A1%A8%E9%A9%B1%E5%8A%A8%E6%B3%95/15768054?noadapt=1)(Table-Driven Approach)，简单讲是指用查表的方法获取值。

表驱动法是一种编程模式，表里可以存数据，也可以存指令，或函数等都可以。

在数值不多的时候我们可以用逻辑语句(if/else 或 case do)的方法来获取值,但随着数值的增多逻辑语句就会越来越长,此时表驱动法的优势就显现出来了。

表驱动编程在 JavaScript 中的一个重要应用是**自动绑定事件**。
### 简单示例
我们平时查字典以及插算数表“九九八十一”就是典型的表驱动法。

```
《乘法算数表》
1 一一得一；
2 二二得四；
3 三三得九；
...
9 九九八十一。

- 第一题：3✖️3 = ？
在上面找。。。好的，就在第三行，我看到它了，结果就是81。
```
### 自动绑定事件
比如下面就是代码中的Controller部分。绑定事件中有jquery的事件监听方法。
```js
const controller = {
    init(x) {
        view.init(x)

        //绑定事件
        controller.bindEvents()
    },
    bindEvents() {
        //jquery对象
        view.element.on('click','#a1',() =>{
            doSomething_a1
        })
        view.element.on('click','#a2',() =>{
            doSomething_a2
        })
        ...
        view.element.on('click','#an',() =>{
            doSomething_an
        })
    }
}
```
上面的代码，有很多重复的地方。用表驱动方法可以简化这些代码。把那些相同相似的地方“隐藏”，只留下需要的。
```js
const controller = {
    init() {
        ...

        //我们将之称为自动绑定事件
        controller.autoBindEvents()
    },
    //事件
    events: {
        'click #a1': 'method_1',
        'click #a1': 'method_1',
        ...
        'click #a1': 'method_n'
    },
    autoBindEvents() {
        //实现过程，比如遍历
        for (let key in controller.events)
        //比如将包含'事件 选择器'的字符串分为几个部分，放到不同的变量中
    },
    method_1() {
        doSomething_a1
    },
    method_2() {
        doSomething_a2
    },
    ...
    method_n() {
        doSomething_an
    }
}
```
在MVC优化代码过程中，我们提取出了一个“哈希表”(events)；**我们只留下了需要的、不能再简略的东西：doSomething。**

这就是表驱动法编程。

## EventBus
EventBus也是一种设计模式或框架，主要用于组件/对象间通信的优化简化。

我们将这样一个抽象概念变的形象化：

**比如有这样的一个需求**：我们在操纵几个不同的 button 时，触发点击事件后会更新数据 data 中的 i ；
而事件监听到只要 i 变化就会重新渲染页面，并将 i 写入页面中的某个位置。
```js
//...view设置完毕
//...model设置完毕
//view中有一个render方法

const controller = {
    ...
    method_1() {
        //model.data.i += 1
        model.data.i = doWithMethod_1
        view.render(model.data.i)
    }
    method_2() {
        model.data.i = doWithMethod_2
        view.render(model.data.i)
    }
    ...
    method_n() {
        model.data.i = doWithMethod_n
        view.render(model.data.i)
    }
}
```
可以有两种方法来单独做一个视图和数据的动态变化：

①使用Vue2的新API：Object.defineProperty;

②使用EventBus。在这里我们简单阐述这种方法。


### 集成方法

使用EventBus，可以将其从外部集成到项目中：

下载到本地或使用远程库。

你也可以自己“造”一个。

### 内部模拟

在JS文件中自己声明一个EventBus，来模拟使用EventBus的功能。为了方便起见，这里通过引入jquery实现。

```js
import $ from 'jquery'

const eventBus = $(window)
```
这里的eventBus包含了很多的方法，**on方法**可以监听事件，**trigger方法**可以触发事件。

**在一个地方使用on方法，另外一个地方使用trigger方法，就初步实现了数据通信功能。**

```js
const model = {
    data: {
        i: come from somewhere
    }，
    //update 事件
    update(data) {
        //将 data 更新到 model.data
        Object.assign(model.data, data)
        //触发 model:updated 事件
        eventBus.trigger('model:updated')
    }
}

const controller = {
    //初始化方法
    init() {
        //初始化渲染
        view.render(model.data.i)
        //只要监听到 model:updated 方法就渲染更新页面
        eventBus.on('model:updated', () => {
            v.render(model.data.i)
        })
    },
    ...
    method_1() {
        //model.update(i: model.data.i + 1)
        model.update(i: model.data.i doWithMethod_1)
    },
    method_2() {
        model.update(i: model.data.i doWithMethod_2)
    },
    ...
    method_n() {
        model.update(i: model.data.i doWithMethod_n)
    }
}
```
### 常用方法与优势
#### 方法
eventBus提供了 on 、off 和 trigger 等API，on 用于监听事件，trigger 用域触发事件。
#### 几个优势
* 使用 eventBus 可以满足“分块”的原则，不需要关心彼此的细节，但是却可以互相调用各自的功能。
* 简洁代码。
* 速度快且轻量。

## 写在最后
本文章希望用通俗易懂的方式阐述MVC和一些思想、方法的关系，具体详细的代码就不予展示。

但是初步接触MVC思想可能会存在误解，欢迎讨论和纠正。