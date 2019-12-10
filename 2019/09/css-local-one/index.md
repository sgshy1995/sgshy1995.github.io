# CSS常见布局与居中


这里，就CSS左右布局，左中右布局，水平与垂直居中，进行讨论。

CSS常用于控制页面布局的定位机制：

**普通流、相对定位、绝对定位和固定定位。还可以使用`float`属性来让元素浮动。**

## 1.左右布局

有以下几种常用方式：

### 浮动定位

最常用的就是通过浮动（左浮或右浮）来实现浮动。

`float`属性允许你将普通流中的元素在它的包含元素内**尽可能**的向左或向右排列。

你应该设置`margin`属性来制定浮动元素之间的间距。

不同元素的高度和宽度不同，为防止浮动元素的后一元素自动上浮，可以**为父元素赋予clearfix类**来清除浮动解决这一问题。

```html
<div class="parent clearfix">
  <div class="descendant1">left</div>
  <div class="descendant2">right</div>
</div>
```

同时在CSS中关于伪类作出声明：

```css
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
```

设置左浮动（或右浮动），将两个子元素左右并排实现布局：

```css
.descendant1,
.descendant1 {
  float: left;
  margin-left: 30px;
}
```

### 绝对定位

还可以通过绝对定位,通过元素脱离文档流来实现。

```html
<div class="parent">
  <div class="descendant1">left</div>
  <div class="descendant2">right</div>
</div>
```
----
```css
.parent {
  position: relative;
}
.descendant1 {
  position: absolute;
  left: 0;
  top: 0;
}
.descendant2 {
  position: absolute;
  left: 60px;
  top: 0;
}
```

## 2.左中右布局

我们类比左右布局，在此基础上实现由两个元素变为三个元素的布局。

### 通过浮动实现：

```html
<div class="parent clearfix">
  <div class="descendant1">left</div>
  <div class="descendant2">center</div>
  <div class="descendant3">right</div>
</div>
```
---
```css
.descendant1,
.descendant2,
.descendant3 {
  float: left;
  margin-left: 20px;
}
```

同理，第一个元素左浮，第三个元素右浮；同时设置三个元素为内联元素：

```css
.descendant1 {
  float: left;
  margin-left: 20px;
  display: inline-block;
}
.descendant2 {
  margin-left:20px;
  display: inline-block;
}
.descendant3 {
  float: right;
  margin-right: 260px;
  display: inline-block;
}
```

### 通过绝对定位：

```html
<div class="parent">
  <div class="descendant1">left</div>
  <div class="descendant2">center</div>
  <div class="descendant3">right</div>
</div>
```
---
```css
.parent {
  position: relative;
}
.descendant1 {
  position: absolute;
  left: 0;
  top: 0;
}
.descendant2 {
  position: absolute;
  left: 60px;
  top: 0;
}
.descendant3 {
  position: absolute;
  left: 120px;
  top: 0;
}

```

## 3.水平居中

### 内联元素居中：

```css
text-align: center;
```

### 块级元素居中：

如果想让一个元素水平居中，首先确定你已经给元素设定了一个宽度（否则将溢满整个屏幕）。

可以通过设置左右的外边距为`auto`值来居中（包括图片）。

```html
<body>
  <p class="text">You can go to everywhere.
    <br>If you like.
  </p>
</body>
```
---
```css
p.text {
  max-width: 300px;
  text-align: center;
  margin: 0 auto;
}
```

## 4.垂直居中

有如下HTML结构：

```html
<p class="text">
    button
</p>
<div class="wrapper">
  <div class="main">
      <img src="./1.jpg" alt="1" width=400 height=400>
  </div>
</div>
```

### 内联元素居中

在单行文本或按钮、小图中的文字很使用的方法，即**设置行高与元素高度一致**即可。

```css
p.text {
  height: 60px;
  line-height: 60px;
}
```

也可以根据实际需要尺寸，为所在元素设置上下的`padding`来实现居中:

```css
p.text {
  padding: 15px 0;
  line-height: 60px;
}
```

### 块级元素居中

可以使用`flex`属性，使元素上下左右全部居中：

```css
.wrapper{
  width: 600px;
  height: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

## 5.其他的方式

**实现一个元素样式有千千万万**，这里还有一个实现居中的技巧，使用CSS3的属性：

```html
<div class="wrapper">
  <div class="main">
  </div>
</div>
```
---

首先绝对定位实现后：

```css
.wrapper{
  width:400px;
  height:400px;
  position:relative;
}
.main{
  width:200px;
  height:200px;
  background:red;
  position:absolute;
}
```

### 左右居中的思想

距离父元素左边一半，但这个对齐方式是以子元素的左边界和父元素的中线对齐；将子元素左移自身的50%即可。

```css
.main{
  left:50%;
  transform: translateX(-50%);
}
```

### 上下居中同理

距离父元素上面50%，将子元素下移自身的50%。