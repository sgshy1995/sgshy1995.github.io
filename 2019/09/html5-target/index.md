# HTML常用标签详解


对于很多HTML初学者来说，记忆和掌握标签是一门很难搞的功课。来详细介绍常用的HTML标签。

### stlye标签

用于定义元素的CSS样式

```HTML
<style>
  /*这里写css样式*/
</style>
```

### script标签

用于定义页面的JavaScript代码，也可以引入外部的JavaScript文件。

```HTML
<script>
  /*这里写JavaScript代码*/
</script>
```
### link标签

引入外部样式css文件。

```html
<link rel="stylesheet" href="css/style.css">
```
### HTML注释

又叫注释标签。

对关键代码注释是一个良好的习惯。在实际开发中，对功能模块代码进行注释尤为重要。

```HTML
<!-- 注释的内容 -->
```
### h(标题)标签

共有六个级别的标题标签：h1、h2、h3、h4、h5、h6。其中h是header的缩写。

这里要注意，一个页面一般只有一个h1标签，表示页面的大标题。

```HTML
<h1>hearder 1</h1>
<h2>hearder 2</h2>
...
```
### p(段落)标签

我们可以用来显示一个段落的文字。

```HTML
<p>Paragraph</p>
```
### br标签

对文字字符进行换行处理。

```HTML
<br>
```
### hr标签

加入水平分割线。

```html
<hr>
```
### strong标签

语义强调，表示重视。同时字符加粗。

```html
<strong>xxx</strong>
```
### div标签

全称“division(分区)”，用来划分一个区域。div标签内部可以放入所有其他标签，例如`p`标签、`strong`标签等。

```html
<div class="">
  <p> I love <strong>study</strong>. </p>
</div>
```
### span标签

段落式标签，和div非常像。但是div是块级元素，而span是内联元素。

```html
<span>xxx</span>
```
### 空格标签

表示一个空格。

```html
&nbsp;
```
### ol标签

有序列表标签。默认以数字顺序。`<li></li>`表示一个列表项。

```html
<ol>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ol>
```
### ul标签

无序排列标签，即unordered list。

```html
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```
### dl标签

定义列表，即definition list。引入两个概念，dt和dd。

`dt`即definition term(定义名词)，用于添加要解释的名词。

`dd`即definition description(定义描述)，用于添加该名词的具体解释。

```html
<dl>
  <dt>term</dt>
  <dd>description</dd>
</dl>
```
### table标签

一个表格一般由几个部分组成。

表格：`table`标签

行：`tr`(table row)标签

单元格：`td`(table data cell)标签

标题：`caption`标签

表头单元格：`th`(table header cell)标签

一般为了使表格语义化结构更清晰也更有可读性和维护性，引入`thead`、`tbody`、`tfoot`三个标签，把表格划分为三部分：表头、表身、表脚。

```html
<table>
  <caption>xxx</caption>
  <thead>
    <tr>
      <th>xxx</th>
      <th>xxx</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>xxx</td>
      <td>xxx</td>
    </tr>
    <tr>
      <td>xxx</td>
      <td>xxx</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>xxx</td>
      <td>xxx</td>
    </tr>
  </tfoot>
</table>
```
### img标签

用来显示图片。有三个属性值：`src`、`alt`、`tittle`。

```html
<img src="图片路径" alt="无法加载时显示的文字" tittle="鼠标移动显示的文字">
```
### a标签

超链接标签。主要有两个属性值：href和target。

```html
<a href="链接地址" target="打开方式">
```
target属性取值：

`_self` 默认值，在当前窗口打开链接

`_blank` 在新窗口打开链接

`_parent` 在父窗口打开链接

`_top` 在顶级窗口打开链接

### form标签

表单标签。

```html
<form name="" method="">
  /*各种表单标签*/
</form>
```

一般常用的属性有name和target两个。

在一个页面中一般不止一个表单。为了区分这些表单，我们使用name来对表单进行命名。每个form标签对应一个表单。

method属性用于指示表单数据使用哪一种http提交方法。取值有两个：`get`和`post`。一般使用`post`，向服务器提交时加密，安全性高。

### input标签

多数表单都用input标签来实现，是自闭合标签。

```html
<input type="">
```

下面介绍标签的几种type属性。

#### text
单行文本框。
```html
name:<input type="text" value="默认显示的字符">
```

#### password
密码文本框。一种特殊的单行文本框，输入时不可见。

```html
password:<input type="password" size="最多可输入字符数">
```

#### radio

单选框。name属性表示单选按钮所在组名，value表示按钮取值。

```html
Gender：
<input type="radio" name="gender" value="man"> Male
<input type="radio" name="gender" value="woman"> Female
```

#### checkbox

复选框。

checked属性表示默认选取。

```html
Fruit:
<input type="checkbox" name="fruit" value="apple" checked> Apple
<input type="checkbox" name="fruit" value="banana"> Banana
<input type="checkbox" name="fruit" value="lemon"> Lemon
```

#### button

普通按钮，一般配合JavaScript进行操作。

```html
<input type="button" value="">
```

#### submit

提交按钮。

```html
<input type="submit" value="">
```

#### reset

重置按钮，只可以重置同一表单中的输入内容。

```html
<input type="reset" value="">
```

### button标签

和input标签中的button实现效果类似，不过此标签在不赋予属性时会自动升级为提交按钮。

```html
<button type="">xxx</botton>
```

### textarea标签

多行文本框。默认文本是在标签内部设置的，而不是在value中。rows和cols属性可以调整文本框的行数列数，但一般使用css来设置宽高。

```html
<textarea rows="" cols="" value="">默认内容</textarea>
```
### select/option标签

下拉列表，以select和option两个标签配合完成。

select属性有：

`multiple` 设置下拉列表可以选择多项。

`size` 设置下拉列表显示几个项，取值整数。

option属性有：

`selected` 设置该选项默认。

`disabled` 设置该选项不可选。

```html
<select multiple size="4">
  <option>xx</option>
  <option selected>xx</option>
  <option>xx</option>
  <option disabled>xx</option>
  <option>xx</option>
  <option>xx</option>
</select>
```

### iframe标签

可以用来实现内嵌框架，以及配合超链接来使用。

```html
<iframe src="http://" width="" height="" name="">
</iframe>
```

或者

```html
<iframe src="xxx" width="" height="" name="">
<a href="xxx" target="_blank"></a>
</iframe>
```

