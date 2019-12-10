# CSS技巧一：IconFont使用


在我们使用CSS对页面进行编辑布局时，经常会用到一些小图标或者logo。

常见的比如我们在制作个人主页时，使用超链接图标来跳转到你的微博或其他页面等。

不外乎两种：

自己通过专业的软件来切图或绘制，需要有一定的了解；

第二种就是找现成的啦。我们在写CSS时需要用到很多的工具或者工具类网站，今天就为大家介绍一个专门采集各种图标的网站和使用的教程：

[阿里巴巴矢量图标库](http://www.iconfont.cn)

## 登录与搜索

首先进入主页，点击右上角的登录，推荐使用GitHub登录。

![登录](/images/css-jiqiao/1.png)

在搜索框内输入你想要找到的图标，比如："`weibo`"。

![搜索](/images/css-jiqiao/2.png)


## 添加入库/项目

出现了很多关于微博的图标，这里我们点击第三行第一个"weibo"，鼠标挪动选择第一个“添加入库”。

已经被添加入库的图标会以虚线框起来，以鉴别是否重复选取。

![微博](/images/css-jiqiao/3.png)

我们再搜索几个自己需要的图标并以此添加入库。

![入库](/images/css-jiqiao/4.png)

点击右上角的购物车小图标，选择添加至项目。创建项目并加入此新项目。

![项目](/images/css-jiqiao/5.png)

## 生成代码

在我的项目页面，「选择格式"`Symbol`"」——「查看在线链接」——「暂无代码，点此生成。」

![图片描述](/images/css-jiqiao/6.png)

## 引入链接

将代码引入html页面中，一般放在`head`标签内即可。

```html
<script src="////at.alicdn.com/t/font_959581_uir2sooudm.js"></script>
```

这是一种最新的引入方式，也是未来主流的方式。

再加入通用CSS代码，一次即可。

```html
<style type="text/css">
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>
```

## 加入页面

这时候就可以通过添加`svg`标签放在合适的位置，来`代替img标签`引用图标了。
还可以通过`font-size`,`color`来调整样式。

```html
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
```
我们可以点击图标浮动信息底部的“`复制代码`”来`替换"#icon-xxx"`。

![复制代码](/images/css-jiqiao/7.png)


## 注意事项

在你对项目中的图标进行操作后，需要重新生成`script标签`并重新引入页面。包括但不限于增加删除、去色上色、修改名字等。

可以通过`width;height`来控制大小，默认是内联元素。

可以通过`fill`属性来控制标签的颜色，但是在某些情况下可能会冲突时效。

[一个失效情况（转自BobChen）](https://juejin.im/post/57975c0c5bbb500063f5ac9d)