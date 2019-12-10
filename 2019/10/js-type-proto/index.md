# 快速理解JS的原型与原型链


## 概述
JavaScript中有基本类型和复杂类型的区分。

当我们在声明一个基本类型时：
```js
var n1= 1
console.log(n1)
//1
```
这时我们可以用**Number**方法将1包装为对象，即声明一个对象1。
```js
var n2= new Number(1)
console.log(n2)
//Number {1}
//[[PrimitiveValue]]:1

//△__proto__: Number

//constructor: ƒ Number()
//toExponential: ƒ toExponential()
//toFixed: ƒ toFixed()
//toLocaleString: ƒ toLocaleString()
//toPrecision: ƒ toPrecision()
//toString: ƒ toString()
//valueOf: ƒ valueOf()
//__proto__: Object
```
n2的**PrimitiveValue**（初始值）为1，其实此时它就是一个**hash**。

此时对象n2现在有很多方法，可以调用对应各自的函数。

## Like Java ?

但是，我们发现有一点：在我们直接声明基本数据类型n1时，也可以使用这些方法调用函数。

比如`toString`方法：
```js
var n1= 1
n1.toString()
//"1"
```
这就要涉及到JavaScript的发明历史。在设计之初，它被要求：**“长得像” Java**。

当时的Java声明一个对象，就是如此：
```java
var n= new Number(1)
```
设计师在设计的同时，为了简便快捷，也制定了我们最常用的声明方式：
```js
var n= 1
```
当然，这种方法肯定被JavaScript程序员所喜爱，第一种方法几乎没有人用。

但是有一个问题，如果直接声明n，那么它就是一个数字。**而基本数据类型是没有属性的。**

这时候该如何调取各种方法呢？

声明一个临时对象，我们暂将它称为**temp**。
比如在对n进行`toString`方法引用时，会声明一个临时对象，对n进行复杂封装，将其变为对象。
```js
var n= 1
n.toString()
```
其实是:
```js
var temp= new Number(n)
temp.toString()
//1
```
将**temp.toString**的值赋予`n.toString`。在操作结束后，临时对象将从内存中抹除。
## 共用属性
number，string，boolean，object都有一些共同的方法，比如`toString`，`valueof`等等。为了避免重复声明和内存浪费，这些方法“归纳”与一个**共用属性**之中。

JavaScript在声明一个对象后，并不是先默认复制一遍共用属性在内存中的存放地址，再在引用时调取对应的函数。

而是：

在声明对象时，就存入一个隐藏的属性：**__proto__**。该属性，对应其共用属性。
```js
var obj= {
  name: 'Jack',
  age: 18
}
console.log(obj)
//△{name:"Jack", age:18}
  //age: 18
  //name: "Jack"
  //△__proto__: Object
    //constructor: ƒ Object()
    //hasOwnProperty: ƒ hasOwnProperty()
    //isPrototypeOf: ƒ isPrototypeOf()
    //propertyIsEnumerable: ƒ propertyIsEnumerable()
    //toLocaleString: ƒ toLocaleString()
    //toString: ƒ toString()
    //valueOf: ƒ valueOf()
    //__defineGetter__: ƒ __defineGetter__()
    //__defineSetter__: ƒ __defineSetter__()
    //__lookupGetter__: ƒ __lookupGetter__()
    //__lookupSetter__: ƒ __lookupSetter__()
    //get __proto__: ƒ __proto__()
    //set __proto__: ƒ __proto__()
```
那我们在调用`toString`方法时:

JavaScript首先会检查数据类型是否是对象；若不是，则包装为对象作临时的转换。

**然后，在检查对象中是否有toString这个属性；若没有，才进入共用属性进行检查。**

两个空对象是不相等的，但他们的共有属性是相等的。
```js
var obj1= {}
console.log(obj1)
//{}
var obj2= new Object()
console.log(obj2)
//{}
obj1 === obj2
//false
obj1.__proto__ === obj2.__proto__
//true
```
但是，当我们声明一个number为对象时，它就有自己区别于普通对象的独特的共有属性。
比如`toFixed`，`toExponential`等等。
那么它的`__proto__`属性就对应自己独有的共同属性，在共同属性中还有另一个隐藏属性`__proto__`对应一般对象的共有属性。这样，number类型的对象就可以调用所有的函数。
## 原型与原型链
这里，就引入了两个新的概念。

**那么，这个共有属性，就被称为原型（对象）。原型对象就是用来存放声明对象中共有的那部分属性。**

JavaScript中所有的对象都可以继承其原型对象的属性。而原型对象自身也是一个对象，它也有自己的原型对象。

**这样层层上溯，就形成了一个类似链表的结构，这就是原型链。**

为了避免对原型对象在没有被使用时被内存清理，JavaScript通过**prototype**来默认引用。

即`Object.prototype`。
```js
Object.prototype

//constructor: ƒ Object()
//hasOwnProperty: ƒ hasOwnProperty()
//isPrototypeOf: ƒ isPrototypeOf()
//propertyIsEnumerable: ƒ propertyIsEnumerable()
//toLocaleString: ƒ toLocaleString()
//toString: ƒ toString()
//valueOf: ƒ valueOf()
//__defineGetter__: ƒ __defineGetter__()
//__defineSetter__: ƒ __defineSetter__()
//__lookupGetter__: ƒ __lookupGetter__()
//__lookupSetter__: ƒ __lookupSetter__()
//get __proto__: ƒ __proto__()
//set __proto__: ƒ __proto__()
```
`Number.prototype`
```js
Number.prototype;

//constructor: ƒ Number()
//toExponential: ƒ toExponential()
//toFixed: ƒ toFixed()
//toLocaleString: ƒ toLocaleString()
//toPrecision: ƒ toPrecision()
//toString: ƒ toString()
//valueOf: ƒ valueOf()
//__proto__: Object
//[[PrimitiveValue]]: 0
```
如此，还有：
`String.prototype`
`Boolean.prototype`
等等。

`Object.prototype.__proto__ === null`

我们制作一个简单的示意图，以便更直观地理解这些概念。

![图片描述](/images/js-type/1.jpeg)

那么我们可以得到这些关系：

```js
var obj= {};
obj.__proto__ === Object.prototype;
//true

var n= new Number(1);
n.__proto__ === Number.prototype;
//true
n.__proto__.__proto__ === Object.prototype;
//true
```

## 结论

总结起来，得到的结论就是：

 - **Object.prototype**是**Object**的共用属性

 - **obj.__proto__**是**Object**的共用属性的引用

 - 同理，对于**String**、**Boolean**、**Symbol**和**Number**也是如此

 ```js
var object = {}
object.__proto__ ===  Object.prototype  // 为 true

var fn = function(){}
fn.__proto__ === Function.prototype  // 为 true
fn.__proto__.__proto__ === Object.prototype // 为 true

var array = []
array.__proto__ === Array.prototype // 为 true
array.__proto__.__proto__ === Object.prototype // 为 true

Function.__proto__ === Function.prototype // 为 true
Array.__proto__ === Function.prototype // 为 true
Object.__proto__ === Function.prototype // 为 true

true.__proto__ === Boolean.prototype // 为 true

Function.prototype.__proto__ === null // 为 true
 ```
