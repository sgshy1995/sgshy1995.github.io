# CSS技巧二：布局细节


在我们写CSS代码的过程中养成记录一些小技巧的习惯。
不然我深有体会的就是，当时觉得**很简单很简单很简单**的东西，过段时间可能需要走弯路用几个小时去实现，这就是蛋疼的CSS吧。

## 修改默认样式

一般特定的元素都有自己默认的样式，但是在我们的整体布局中，可能适得其反。

我习惯使用**border-box**的盒模型，因人而异。

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

a {
  color: inherit;
  text-decoration: none;
}

ul,
ol {
  list-style: none;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: normal;
}
```
等等。

## 没有border怎么做CSS？

在调试一个盒子的时候，我们可以为其加上边框。更易于直观地看出盒子的位置以及样式。

```html
<div class="bd"></div>
```
---
```css
.bd {
  border: 1px solid red;
}
```

## 善用开发者工具

要善用Chrome开发者工具检查网页代码，猜就是浪费时间。

可以在网页结构上直接作出暂时性的修改，查看每个元素的样式。

关于这点我们在之后的博客进行详细说明。

## 尽量少定义宽高

尽量少用 **width** 和 **height** 这两个属性。

**在定义元素的高度时，我们知道一个元素的高度是由内容决定的，比如div高度由其内部文档流高度总和决定。**

这样直接用width输入像素值，很容易造成内部溢出而显示错误或超出范围。

在定义元素高度时，如果直接用 **height** 输入像素也会因一些变化或其他CSS样式而引起显示问题。不到万不得已，尽量少使用或在可控范围内使用。

我们可以根据实际大小位置需要，用 **margin** 和 **padding** 来控制元素的位置以及样式大小。

## 不到万不得已

如果要规定div的宽度，尽量使用 **max-width** 。可以在缩小浏览器窗口时等比例缩放。
```css
max-width: 600px;
```

## 固定定位的问题

一般在制作导航栏等固定部件时会用到固定定位，使该元素脱离文档流，可以相对于屏幕固定。

```html
<div class="parentNav">
  <div class="topNavBar">
  </div>
</div>
```

但是固定定位会导致元素样式收缩，可以用 **width: 100%;** 来解决。

但此时不能加左右的padding，否则总宽度会超过body的宽度。

**解决办法是给一个父元素div 可以用来调整 padding 和 margin 。div 宽度无法超过 body ，而且会自适应。**

```css
.parentNav {
  padding: 0 6px;
}
.parentNav .topNavBar {
  position: fixed;
  top: 0;
  left: 0;
  padding: 24px 0;
  width: 100%;
}
```

## dt和dd左右布局
如何实现 dl 中的 dt 和 dd 左右布局？

将`<dl>`中的`<dt>`和`<dd>`一起设置通向浮动后，发现所有元素并排排在一起；虽然自动换行，但是是无序的。

我们为达到目的：让同一类左右排布；不同类上下排布。

**可以利用自动换行的原理，为`<dt>`和`<dd>`分别设置百分比宽度，总和达到100%即可。**

同时用 padding 调节上下间距。

```html
<section class="text">
  <dl>
    <dt>Age: </dt>
    <dd>20</dd>
    <dt>Gender: </dt>
    <dd>Male</dd>
    <dt>Height: </dt>
    <dd>180cm</dd>
  </dl>
</section>
```
---
```css
.text dl dt {
  float: left;
  width: 30%;
  padding: 6px 0;
}
.text dl dd {
  float: left;
  width: 70%;
  padding: 6px 0;
}
```
## 元素hover的动画问题

当遇到配合JS实现样式变化时，比如在一个元素hover时出现，即 **dispaly:block** ，在鼠标移走后变为 **none** 。

这时候，它的 **transition: time** 就会失效。

可以换一种思路，比如说父级元素设置 **overflow:hidden**，子元素默认移除父元素，鼠标进入元素时移动回原位。

但是如果想要配合其他元素的动画实现，避免互相影响，可能需要配合JS使用。

---
以后会补充一些小技巧，持续更新。