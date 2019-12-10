# 从不断“进化”的源码来碰面jQuery


## 其本质
jQuery的本质是什么？一个JavaScript库。

接受一个旧的节点，返回一个新的对象（哈希），这个对象就是jQuery对象。

jQuery的特点就是它拥有自己的API。

**jQuery的源码调用DOM API，但是jQuery对象无法使用DOM API。**
## 便利之处
jQuery极大地简化了JS编程，也容易上手和学习。

尽可能的避免了你使用原生DOM那些很“烂、复杂”的API。

jQuery的兼容性极好。

但是极多的API和不断更新的时效性，想要透彻了解还是离不开：熟能生巧。
## 实现过程
其实就是编写源码，反复优化实现需求的过程。
### 封装函数
首先我们来封装函数。
这里只举一个“添加类”的函数为例。在实际开发中，我们要声明很多的函数来实现方法的需求。

我们使用简单的html结构：
```html
<body>
    <div>111</div>
    <div>222</div>
    <div>333</div>
    <ul>
        <li id="item1">select1</li>
        <li id="item2">select2</li>
        <li id="item3">select3</li>
    </ul>
</body>
```
函数的功能：

为指定节点(node)添加类。
```javascript
function addClass(node,classes) { //给传入的元素节点添加一个或多个类
  classes.forEach((value)=>node.classList.add(value))
}

addClass(item3,['a','b']) //id为"item3"的节点

//<li id="item3" class="a b">select3</li>
```
### 命名空间
上面的函数，虽然功能已经初步实现，但是有一个很大的问题：

我们在处理复杂页面时，为了方便使用和记忆，可能会声明很多名字和DOM API极为“类似”的函数；那么就会在不知不觉中，覆盖掉一些API固有的函数，也可能会覆盖一些全局变量。

DOM API将众多方法放在document对象中(`window.document.xxx`)。我们也需要给它们一个命名空间，即将其写入一个库中，来暂时性的解决这个问题。

也同时直接的告诉开发者：**我们声明的这些函数，是有内在的关联性的，因为它们都是在操纵节点。**

**命名空间**：名字空间，也称命名空间、名称空间等，它表示着一个标识符的可见范围。一个标识符可在多个名字空间中定义，它在不同名字空间中的含义是互不相干的。命名空间就是一种设计模式。（详见[维基百科](https://zh.wikipedia.org/wiki/%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)）

比如笔者的 name = 'sgs'
```javascript
window.sgs = {} //声明一个空的库

function addClass(){如上}
sgs.addClass = addClass

sgs.addClass(item2,['a','c'])
//实现同样的效果
```
这样我们就可以调用sgs库的方法来达到同样的目的，而避免了全局变量的覆盖；同时，这些方法都在我们归纳的同一个库中。

这个问题解决以后，我们又发现：我们其实并不想这样使用一个API。

**我们是否可以直接的对节点调用一个功能函数：item3.addClass()呢？**
### 直接操纵节点
如何实现？将node放在“前面”。
```js
node.fn(x,y)
```

我们来“篡改”Node的原型，给Node的公有属性添加一个属性。

把函数中的**node**全部替换为**this**，调用函数时默认把**this**当做第一个参数传入进来。
```js
Node.prototype.addClass = function(classes) { 
  classes.forEach((value)=>this.classList.add(value))
}
```
我们可以在Node.prototype中找到我们自己添加的属性。

使用方法时：
```js
item3.addClass(['a','b','c'])
//=== item3.addClass.call(item3,['a','b','c'])
```

但是，我们这样操作是在改Node的公有属性。这样就有很大的麻烦：

如果众多开发者都在改公有属性，那么既会相互覆盖，同时公有属性也失去了本身最为一个标准的意义。

如何再进一步的改进？

### 重要的替换
我们自己声明一个类似的"Node"，名为：**"jQuery"**

```js
window.jQuery = function(node){ //传入node节点
  return {
            //key: value
            
            node.APIOne: function(){}
            node.APITwo: function(){}
            ...
            node.addClass: function(classes){
               classes.forEach((value)=>node.classList.add(value))
            }
         }
}
```
我们现在是这样使用的

```js
//首先根据节点获取一个新的对象
var nodex = jQuery(item3)

//使用jQuery提供的新API来操作 
nodex.addClass(['a','b']) 
```
首先我们把参数item3传给了jQuery，然后jQuery将其存在了函数的node里面；这样我们使用其他方法时，可以直接通过操作nodex这个对象来操作节点。

### 接受选择器
当你传入的不是一个id对应的节点，而是表示一个节点的选择器：
```js
var nodex = jQuery('#item3')
```
让jQuery源码进行传参类型的判断：
```js
window.jQuery = function(nodeOrSelector){
  let node
  if(typeof nodeOrSelector === 'string'){
    node = document.querySelector(nodeOrSelector)
    //如果jQuery发现你输入了字符串，那么就回去寻找这个字符串对应的节点
  }else{
    node = nodeOrSelector
  }
  return
  ...
}
```
### 多元素选择器
当你传入一个选择多个元素节点的选择器时
```js
var nodex = jQuery('ul>li')
var nodey = jQuery('ul li:nth-child(2)')
```
将1~n个节点全部放在伪数组中。
```js
window.jQuery = function (nodeOrSelector) {
    //--------------------判断部分--------------------
    let nodes = {}
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector) //伪数组，但它的原型链还覆盖有很多层
        for (let i = 0; i < temp.length; i++) { //获取纯净的原型链，__proto__===Object.prototype
            nodes[i] = temp[i]
        }
        nodes.length = temp.length
    } else if (nodeOrSelector instanceof Node) { //说明传入的是一个节点，但是保持一致性也要变成伪数组
        nodes = {
            0: nodeOrSelector,
            length: 1
        }
    }
    
    //--------------------添加类方法--------------------
    nodes.addClass = function () { 
        let array = [] //传入多个字符串代表类，不用传入数组，内部将字符串组合为数组
        for (let k = 0; k < arguments.length; k++){
            array.push(arguments[k])
        }
        array.forEach((value) => {
            for (let i = 0; i < nodes.length; i++) { //nodes并不是元素，而是伪数组
                nodes[i].classList.add(value)
            }
        })
    }
    
    //------------------设置节点文本方法------------------
    nodes.setText = function (text) { //设置传入参数的文本
        for (let i = 0; i < nodes.length; i++) {
            nodes[i].textContent = text
        }
    }
    
    //-------------------------------------------------
    return nodes
}

//** 我们同时引入$，通过$来声明jQuery对象：**

window.$ = jQuery
```
使用jQuery的两个API：
```js
var $div = $('div')
var $li = $('ul>li')
$div.addClass('active')
$li.setText('We are the same')
```
## 写在最后

至此，可以看出，jQuery的本质就是一个JavaScript库。

通过一个**进化的过程来达到一个使用的标准**。

jQuery的API就是源代码中的属性对应的函数，而且jQuery的源码远比此复杂和繁琐，才得以实现如此强大的功能。

我们可以允许在不完全透彻理解的情况下使用一个原理，**就像我们引入外部的库一样。**

我们在此只是形象化的了解jQuery实现的基本原理。知根知底，才可以更透彻的学习和使用jQuery。
