# 浅谈：JS中的数据类型


JS中的数据类型。什么是数据类型？

和我们平时交流一样一样。

什么是汉字，什么是拼音，什么是标点符号，什么又是现在流行的表情包？类型，可以是语言的分类，也一定是理解一门语言或了解一个事物的基础。

JavaScript中的每一个“值”，都是一种数据，属于一种数据类型。

我们常用的数据类型，有六种：

 - **number**：数值，包括整数与小数
 - **string**：字符串，理解为文本
 - **boolean**：布尔值，只有两个值：**true**和**false**，分别表示真假
 - **undefined**：未定义或不存在
 - **null**：空值，此处为空
 - **object**：对象，各种值的集合
 - 在ES6中引入了第七种类型的值：**symbol**

数值、字符串和布尔值一般被称为原始类型，是最基础的类型；对象被称为合成类型，是比较复杂的类型。

**undefined**和**null**是特殊的值。
### 数值
JavaScript的数值有许多种表示类型。

常规的十进制：首数字不为0。 **520**
二进制：前缀为**0b**或**0B**。 **0b101**
八进制：前缀为**0o**或**0O**。 **0o363**
十六进制：前缀为**0x**或**0X**。 **0xdd**
科学计数法：字母e或E后面接整数表示指数部分。**314e-3**

**NaN**是一种特殊的数值，表示"非数字"。
### 字符串
字符串就是排在一起的多个字符，用英文**双引号 " 或单引号 ' 表示**。

双引号内部可以引用单引号，单引号内部也可以引用双引号。

当引号使用出现歧义时，比如单引号中的单引号，双引号中的双引号，就要用到**转**。

**反斜杠** \ 可以用来表示一些特殊字符，被称为**转义符**。

常见的转义符有：
 - `\n`：换行符
 - `\t`：制表符
 - `\r`：回车键
 - `\b`：后退键
 - `\'`：单引号
 - `\"`：双引号
 - `\\`：反斜杠
### null 和 undefined
对于**null**和**undefined**，都可以表示没有，含义相似。这也与历史上JavaScript的设计有关。

大致可以像下面这样理解。

**null**表示空值，即该处的值现在为空。
比如调用函数时，某个参数未设置任何值，这时就可以传入**null**，表示该参数为空。

**undeined**则表示此处未定义，不存在。
### 布尔值
**true**为真，**false**为假。
布尔值运算关系有 **“与”：&& 和 “或”：||**，真值关系符合标准真值表。
#### falsy值
 - false
 - 0
 - '' , "" , ``
 - null
 - undefined
 - NaN

### 对象
这是JavaScript中最核心的概念，也是最复杂的数据类型。

对象就是一组“键值对”即**key-value**的集合，是一种**无序**的复合数据集合。

```js
var obj= {
  k1 : "My",
  k2 : "Love",
  k3 : 18,
  k4 : undefined,
  k5 : {k6 : "998"}
};
```

每一个键值对用**逗号,隔开。**

冒号左边为键名，默认为字符串，加不加引号都可以。

但若键名不符合标识名规则，则必须加上引号，否则会报错：**"3+4"** **"678xyz"**。

**对象的每一个键名又称为属性，它的键值可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为方法，它可以像函数那样调用。**

读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。
```js
obj.k1
obj["k1"]
```

如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。
这种方法不仅可以用来读取值，还可以用来赋值。

```js
var obj = {}
obj.k1 = "998"
obj["k2"] = undefined
```

可以使用`Object.keys`来查看一个对象本身的所有属性。

```js
var obj = {
  k1 : "uux",
  k2 : 18
}

Object.keys(obj);
//["k1","k2"]
```
可以用`delete`命令删除对象的属性，删除成功后返回true。
```js
var obj = {
  k1 : "123",
  k2 : "234"
};

delete obj.k1;   //true
obj.k1;   //undefined
Object.keys(obj);   //["k2"]
```
`in`用于检查对象是否包含某个属性，如果包含就返回true，否则返回false。
```js
var obj = {};

"k1" in obj;   //false
```

`for...in`循环用来遍历一个对象的全部属性。
```js
var obj = {
  k1 : "Male",
  k2 : 18, 
  k3 : undefined
};

for (var i in obj) {
  console.log("Key:", i);
  console.log("Value:", obj[i]);
}
// Key:k1
// Value:"Male"
// Key:k2
// Value:18
// Key:k3
// Value:undefined
```
### typeof运算符
`typeof`运算符可以返回一个值的数据类型。
```js
typeof 998;   //"number"
typeof "998";   //"string"
typeof true;   //"boolean"
typeof undefined;   //"undefined"
typeof {};   //"object"

var a = {};
typeof a;   //"object"
```
#### 特例：
```js
function xxx() {};
typeof xxx;   //"function"
```
函数返回**function**，但是并没有**function**这一数据类型。
```js
typeof null;   //"object"

```
null返回object？？？**这只是一个BUG**，是由历史原因造成的。
```js
typeof [];   //"object"

```
空数组的类型也是**object**。
```js
typeof document;   //"object"
typeof window;   //"object"
...
```
一些其他的返回类型。