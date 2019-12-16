# Webpack高级：环境配置、引入和懒加载


## 文档

这里介绍webpack的一些高级配置，包括根据不同环境（开发和生产）使用不同的配置文件；引入 SCSS， LESS和 Stylus，以及引入图片和懒加载等。

详见 webpack 的 [英文文档](https://webpack.js.org/guides/getting-started/) 和 [中文文档](https://www.webpackjs.com/guides/getting-started/)。

## 插件与加载器

webpack使用和拓展通过两种方式来实现：加载插件或安装加载器。

### 两者的区别

- **首先本意上的差别。**
- **加载器主要用来加载一个资源文件，插件用来加强、扩展功能。**
- 当然 loader 也变相地扩展了 webpack ，但是它只专注于转化文件。
- plugin 的功能更加丰富，不仅局限与资源的加载。
- **loader 必定是一个文件加载为另外一个文件，而 plugin 可能是综合多个文件合并为一个文件。**

## 引入

### 引入SCSS

引入 .scss 文件。

#### 要求

请使用 dart-sass 代替 node-sass，因为后者已经过时了而且很难安装。

#### 安装

```shell
$ yarn add sass-loader dart-sass --dev
```

下面示意 sass-loader 和 css-loader 、 style-loader 的链式调用。

#### 引入到js

**index.js**

```js
import './style-one.scss'
```

**style-one.scss**

```css
$color: red;

body{
  color: $color;
}
```

#### 配置

sass-loader 默认使用 node-sass ，你需要把它修改为 dart-sass。

**webpack.config.js**

```js
module.exports = {
    ...
    module: {
       rules: [
+        {
+           test: /\.scss$/,
+           use: [
+               "style-loader", // 将 JS 字符串生成为 style 节点
+               "css-loader", // 将 CSS 转化成 CommonJS 模块
+?              //"sass-loader"  将 Sass 编译成 CSS，如果只写这一行，默认使用 Node Sass
+               {
+                 loader: "sass-loader",
+                 options: {
+                   implementation: require('dart-sass')
+                 }
+               },
+           ]
+       }
      ]
    }
}
```

### 引入LESS

引入 .less 文件。

#### 要求

此模块需要 Node v6.9.0+ 和 webpack v4.0.0+。

#### 安装

```shell
$ yarn add less-loader less --dev
```

下面示意 less-loader 和 css-loader 、 style-loader 的链式调用。

#### 引入到js

**index.js**

```js
import './style-two.less'
```

**style-two.less**

```css
@color: red;

body{
  color: @color;
}
```

#### 配置

**webpack.config.js**

```js
module.exports = {
    ...
    module: {
        rules: [
+         {
+           test: /\.less$/,
+           use: [
+               "style-loader", // 将 JS 字符串生成为 style 节点
+               "css-loader", // 将 CSS 转化成 CommonJS 模块
+               "less-loader", // 将 Less 编译成 CSS
+           ]
+         },
        ]
    }
}
```

### 引入Stylus

引入 .styl 文件。


#### 安装

```shell
$ yarn add stylus-loader stylus --dev
```

下面示意 less-loader 和 css-loader 、 style-loader 的链式调用。

#### 引入到js

**index.js**

```js
import './style-three.styl'
```

**style-three.styl**

```css
setColor = red;

body{
  color: setColor;
}
```

#### 配置

**webpack.config.js**

```js
module.exports = {
    ...
    module: {
        rules: [
+         {
+           test: /\.less$/,
+           use: [
+               "style-loader", // 将 JS 字符串生成为 style 节点
+               "css-loader", // 将 CSS 转化成 CommonJS 模块
+               "stylus-loader", // 将 Stylus 编译成 CSS
+           ]
+         }
        ]
    }
}
```

### 使用file-loader

file-loader 加载器可以根据 require 路径来把文件变成文件路径，以达到加载目的。

#### 安装

```shell
$ yarn add file-loader --dev
```

#### 引入图片

首先将图片 logo.png 放入 src 目录的 assets 文件夹中（可以放在src根目录下，根据你的 require 路径）

#### js中引入

**./assets/index.html**

```html
<body>
  <div id="logo"></div>
</body>
```

**index.js**

```js
import png from './one.png'

const logo = document.getElementById('logo')
logo.innerHTML = `
  <img src="${png}">
`
```

#### 配置

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
+     {
+       test: /\.(png|svg|jpg|gif)$/,
+       use: [
+         {
+           loader: 'file-loader',
+           options: {
+             context: 'project',
+             publicPath: 'assets',
+           },
+         },
+       ],
+     },
    ],
  },
}
```

## 懒加载

懒加载或者按需加载，是一种很好的优化网页或应用的方式。

这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块。

这样加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载。

### 实现思路

用 **import( )** 将其当成一个函数去加载文件，得到一个 **promise** ，然后在 **.then** 设置成功后调用得到的结果（可以得到一个函数），或失败后报错等。

### 示例

创建需要懒加载的文件。

```shell
$ touch src/lazy.js
```

**./assets/index.html**

```html
<body>
  <div id="lazy-load"></div>
</body>
```

**index.js**

```js
const div = document.getElementById('lazy-load')
const button = document.createElement('button')

button.innerText = 'I am Lazy-Load'

button.onclick = () => {
  const promise = import('./lazy')
  promise.then((module)=>{
    module.default()
  },()=>{
    console.log('Error')
  })
}

div.appendChild(button)
```

**lazy.js**

```js
export default function lazyModule() {
  console.log('This is a lazy loading module.')
  alert('This is a lazy loading module.')
}
```


## 使用不同的配置文件

在开发模式和生产模式，选择不同的配置，达到使用 webpack 的最优化选择。

在开始之前，你需要先配置 json 文件中的 script 脚本。让不同方法使用不同的配置文件。

**package.json**

```json
{
   "script": {
+    "start": "webpack-dev-server --config webpack.config.dev.js",
+    "build": "rm -rf dist ; webpack --config webpack.config.prod.js"
   }
}
```

### 配置base文件

两种模式共有的配置。

**你要在下面两种模式的配置文件中继承该配置。同时删除默认的配置文件。**

**webpack.config.base.js**

```js
const HtmlWebpackPlugin = require("html-webpack-plugin")
const path = require("path")

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "index.[contenthash].js"
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "Use webpack to load HTML",
      template: "src/assets/index.html"
    })
  ],
  module: {
    rules: [
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: ["file-loader"]
      },
      {
        test: /\.styl$/,
        loader: ["style-loader", "css-loader", "stylus-loader"]
      },
      {
        test: /\.less$/,
        loader: ["style-loader", "css-loader", "less-loader"]
      },
      {
        test: /\.scss$/i,
        use: [
          "style-loader",
          "css-loader",
          {
            loader: "sass-loader",
            options: {
              implementation: require("dart-sass")
            }
          }
        ]
      }
    ]
  }
}
```

### 开发模式

讲究速度快、轻量。比如直接在html文件中插入style标签而不是抽成css文件等。

综合以上，配置如下：

**webpack.config.dev.js**

```js
const HtmlWebpackPlugin = require("html-webpack-plugin")
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const path = require("path")

const base = require('./webpack.config.base.js')

module.exports = {
  ...base,
  mode: "development",
  devtool: "inline-source-map",
  devServer: {
    contentBase: "./dist"
  },
  module: {
    rules: [
      ...base.module.rules,
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"]
      }
    ]
  }
}
```

### 生产模式

最终敲定稿。也是要展示给用户的最终版本。

**webpack.config.prod.js**

```js
const HtmlWebpackPlugin = require("html-webpack-plugin")
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const path = require("path")

const base = require('./webpack.config.base.js')

module.exports = {
  ...base,
  mode: "production",
  plugins: [
    ...base.plugins,
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css',
      chunkFilename: '[id].[contenthash].css',
      // 是否启用删除有关冲突顺序的警告
      // Enable to remove warnings about conflicting order
      ignoreOrder: false, 
    }),
  ],
  module: {
    rules: [
      ...base.module.rules,
      {
        test: /\.css$/i,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../',
            },
          },
          'css-loader',
        ],
      }
    ]
  }
}
```