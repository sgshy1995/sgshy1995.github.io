# 用CSS做一个字母围绕圆转的动画


## 用CSS实现一个旋转动画

周围一圈子母（两串单词“PLAY”）围绕着一个圆圈转动。

如下所示：

![动画](/images/css-roundanimation/1.gif)

大致的思路就是几个`div`用动画控制旋转：

### ①第一步

设置你需要设置四个div写着两个P、两个L、两个A、两个Y。

（我在他们中间加了一个`·`，但是需要**再加一个div做成圆圈**来占据中间的位置，也方便调整大小）。

我给每个div中的文字两个字母之间加了很多个`&nbsp;`来控制字母的间距。你也可以：

在设置`text-align: center`后，设置`letter-spacing`来控制字母的间距。

### ②第二步

给它们默认的旋转位置，比如:

```css
div.one{
  transform: rotate(0);
}
div.two{
  transform: rotate(-45deg);
}
……
```

同时给他们绝对定位，相对于他们的`div.wrapper`。

### ③第三步

调整好专门用来占据中间位置的圆圈的样式。

比如：`border-radius:50%`、`backgroud:black`等。

### ④第四步

给每个div一个旋转动画，对应各自的旋转起点和重点，设置相同的时间，就可以实现同步效果啦。

比如说：

```css
div.one{
  transform: rotate(0);
  animation: 2s one linear infinite;
}
div.two{
  transform: rotate(-45deg);
  animation: 2s two linear infinite;
}
@keyframes one {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
@keyframes two {
  from {
    transform: rotate(-45deg);
  }
  to {
    transform: rotate(315deg);
  }
}
……
```
## 源代码地址

代码及浏览地址：[JSBIN](http://js.jirengu.com/gojonetuwo/1/edit?html,css,output)