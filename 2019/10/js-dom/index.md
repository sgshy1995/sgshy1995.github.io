# 说一些关于DOM


## DOM API
文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展置标语言的标准编程接口。DOM即文档对象模型，是一种处理HTML和XML文件的标准API。

## window
window 对象表示一个包含DOM文档的窗口，window作为全局变量，代表了脚本正在运行的窗口，暴露给 Javascript 代码。

但是同一个窗口的标签页之间不会共享一个 window 对象。

### document
```js
window.document.api()
```
返回对当前窗口所包含文档的引用。

### console
```js
window.console()
```
返回 console 对象的引用，该对象提供了对浏览器调试控制台的访问。

### crypto
```js
window.crypto()
```
返回浏览器 crypto 对象。

### location
```js
window.location()
window.location.href('./')
```
获取、设置 window 对象的 location, 或者当前的 URL。

### scorll
```js
window.scroll()
```
将窗口滚动到文档中指定的位置。

```js
window.scrollY()
```
获取当前滚动位置与视口顶点的距离。

```js
window.scrollX()
```
获取当前滚动位置与视口左点的距离。

### parent
```js
window.parent()
```
返回当前窗口或子窗口的父窗口的引用。

### alert
```js
window.alert()
```
显示警报对话框。

### close
```js
window.close()
```
关闭当前窗口。

### moveto
```js
window.moveTo()
```
将窗口移动到指定坐标。

### 注意
**当你调用一些window的作用域下的函数时，window可以省略。**

window object是所有浏览器都支持的，代表当前窗口。全局变量是window对象的属性，全局函数是window的方法。

一般你不写window，也会默认对象是window。

## Node
node是一个接口，许多 DOM API 对象的类型会从这个接口继承。

它允许我们使用相似的方式对待这些不同类型的对象；比如, 继承同一组方法，或者用同样的方式测试。

### childNodes
```js
Node.childNodes()
```
返回一个包含了该节点所有子节点的实时的`NodeList`。`NodeList` 是“实时的”意思是，如果该节点的子节点发生了变化，`NodeList`对象就会自动更新。

### firstChild
```js
Node.firstChild()
```
返回该节点的第一个子节点Node，如果该节点没有子节点则返回null。

### lastChild
```js
Node.lastChild()
```
返回该节点的最后一个子节点Node，如果该节点没有子节点则返回null。

### nextSibling
```js
Node.nextSibling()
```
返回与该节点同级的下一个节点 Node，如果没有返回null。

### previousSibling
```js
Node.previousSibling()
```
返回一个当前节点同辈的前一个结点( Node) ，或者返回null（如果不存在这样的一个节点的话）。

### parentNode
```js
Node.parentNode()
```
返回一个当前结点Node的父节点 。如果没有这样的结点，这个属性返回null。

### nodeType
```js
Node.nodeType()
```
返回一个与该节点类型对应的无符号短整型的值，可能的值如下：

| Name | Value |
-|:-|
| &nbsp; | &nbsp; |
| ELEMENT_NODE | 1 |
| ATTRIBUTE_NODE | 2 |
| TEXT_NODE | 3 |
| CDATA_SECTION_NODE | 4 |
| ENTITY_REFERENCE_NODE | 5 |
| ENTITY_NODE | 6 |
| PROCESSING_INSTRUCTION_NODE | 7 |
| COMMENT_NODE | 8 |
| DOCUMENT_NODE | 9 |
| DOCUMENT_TYPE_NODE | 10 |
| DOCUMENT_FRAGMENT_NODE | 11 |
| NOTATION_NODE | 12 |

此方法可以配合判断语句来获得我们希望得到的节点，比如说**element**。

示例：

```html
<html>
...
<div id='div1'>哥哥</div> //这里有一个回车
<div id='div2'>弟弟</div>
...
</html>
```
---
```js
var a = document.getElementById('div1')
var b = a.nextSibling
while(b.nodeType !== 1){
  b = b.nextSilbing
}
console.log(b.id)
//'div2'
```

## DOM Document

### Document 对象

每个载入浏览器的 HTML 文档都会成为 Document 对象。

Document 对象使我们可以从脚本中对 HTML 页面中的所有元素进行访问。 同时 Document 对象是 Window 对象的一部分，可通过 window.document 属性对其进行访问。这一点关于window的问题，我们在上面已经提起。

### getElement/s
```js
document.getElementById()
```
返回对拥有指定 id 的第一个对象的引用。
```js
document.getElementsByName()
```
返回带有指定名称的对象集合。
```js
document.getElementsByTagName()
```
返回带有指定标签名的对象集合。

### write
```js
document.write()
```
向文档写 HTML 表达式 或 JavaScript 代码。它允许一个脚本向文档中插入动态生成的内容。

## DOM Element
### Element 对象
在 HTML DOM 中，Element 对象表示 HTML 元素。

Element 对象可以拥有类型为元素节点、文本节点、注释节点的子节点。

NodeList 对象表示节点列表，比如 HTML 元素的子节点集合。

### className
```js
element.className()
```

### id
设置或返回元素的 class 属性。
```js
element.id()
```
设置或返回元素的 id 属性。

### getAttribute
```js
element.getAttribute()
```
返回元素节点的指定属性值。
### 其他对象
除此以外，还有Attr(Attribute)对象、Event对象等。
