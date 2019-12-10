# JS中的数据类型转换


关于JavaScript中的各种数据类型的简单转换。

## 转换为字符串

### toString

可以用`toString`这个API将其他数据类型转换为字符串，其中也有一些特例。

```js
var a = 1;
a.toString;
//"1"

var b = true;
b.toString;
//"true"

```
将数字和布尔值转化为字符串。
```js
var c = null;
c.toString;
//Uncaught TypeError: Cannot read property 'toString' of null

var d = undefined;
d.toString;
//Uncaught TypeError: Cannot read property 'toString' of undefined
```
**null**和**undefined**并没有`toString`这个API，所以会报错。不可以这样使用。
```js
var obj = {age : 18};
obj.toString();
//"[object Object]"
```
`object`的`toString`方法并不能得到你想要的结果。除非你自己设法去编写一个自定义函数。
### 全局方法
使用全局方法`window.String`，等同于`xxx.toString`。
```js
window.String(1);
//"1"

window.String({});
//"[object Object]"
```
**null**和**undefined**也可以使用这个全局方法。
```js
window.String(null);
//"null"

window.String(undefined);
//"undefined"
```
### console.log
`console.log`API可以将数据输出为字符串。
```js
console.log(1);
//1
```
这里console.log中的1其实是"1"，等同于
```js
console.log((1)toString);
//1
```
输出结果的1其实也是"1"，这是在Chrome测试台的输出结果，没有按规则显示而已。

在很多地方，如果需要字符串的数据，会自动调取这个API。

### 常规转换
最简单的转换方式，与空字符串`''`相加`+`
```js
1 + ''
//"1"

true + ''
//"true"

var obj = {}
obj + ''
//"[object Object]"

null + ''
//"null"

undefined + ''
//"undefined"
```
`+`如果左右两边有字符串，那总是会尽可能把结果变为字符串。
```js
'1' + 1
//"11"

(1)toString() + '1'
//"11"
```
这样使用会出现意想不到的结果。两者是等价的。

## 转换为布尔值

### 全局方法
`window.Boolean`方法
```js
window.Boolean(0);
//false

window.Boolean(1);
//true

window.Boolean({});
//true
```
注意要区分空字符串和有空格的字符串。
```js
window.Boolean('');
//false

window.Boolean(' ');
//true
```
如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。

转换规则是除了下面七个值被转为**false**，其他值都视为**true**。
 - `0`
 - `null`
 - `undefined`
 - `NaN`
 - `false`
 - `''`
 - `""`
又叫做**falsy值**。是在Boolean上下文中认定可以转化为**false**的值。
### 常规转换
`!!`取反两次，对应的布尔值不变。
```js
!!0
//false

!!null
//false

!!{}
//true
```
## 转换为数字

### 大概有几种方法

```js
window.Number('105');
//105

window.parseInt('105',10);
//105
//10表示进制

window.parseFloat('1.05');
//1.05

'105' - 0;
//105

+ '105'
//105

+ '-105'
//-105

- '105'
//-105

- (- '105');
//105
```
### 关于parseInt
若不写明进制，则默认为十进制。
```js
parseInt('011');
//11

parseInt('011',8);
//9
```
而且从第一位开始返回，若遇到无法返回的值，则自动结束。

若一位也无法返回，则为**NaN**。
```js
parseInt('1s');
//1

parseInt('ss');
//NaN
```