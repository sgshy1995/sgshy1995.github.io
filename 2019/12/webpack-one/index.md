# Webpack入门：从安装到配置


## 文档

[webpack](https://www.webpackjs.com/) 用于编译 JavaScript 模块。

一旦完成安装，你就可以通过 webpack 的 CLI 或 API 与其配合交互。

这里以入门者的角度（中文）介绍 webpack 的常用配置。当 webpack 更新后，以下细节可能有出入，但是总体的核心不变。

详见 webpack 的 [英文文档](https://webpack.js.org/guides/getting-started/) 和 [中文文档](https://www.webpackjs.com/guides/getting-started/)。

## 基本安装

### 版本信息

首先通过 npm 查看 webpack 目前的版本信息。

```shell
$ npm info webpack
```

可以看到下面的信息：

```js
// webpack@4.41.2 | MIT | deps: 23 | versions: 625

// 可执行文件
// bin: webpack

// 所有版本
// dist-tags:
// latest: 4.41.2   legacy: 1.15.0   next: 5.0.0-beta.9
// webpack-2: 2.7.0   webpack-3: 3.12.0
```

我们使用默认的最新版本 webpack4。这里推荐使用 yarn 代替 npm。

### 初始化安装

新建（进入）项目，初始化 yarn 创建 package.json 后，在本地安装。


```shell
$ mkdir webpack-demo && cd webpack-demo
$ yarn init -y
$ yarn add webpack webpack-cli --dev
```

安装成功后，项目中出现名为 node_modules 的文件夹和名为 yarn.lock 以及 package.json 的文件。

## 创建结构/目录

在根目录下新建 **src 目录** 和 **index.js 文件**。

```shell
$ mkdir src
$ touch src/index.js
$ echo 'console.log('success')' >> src/index.js
```

## 启动 webpack

### 使用本地路径

我们的 webpack 因为是装在本地，所以需要用路径来使用。

```shell
$ ./node_modules/.bin/webpack
```

### 使用 npx 命令

使用 npx 来启动 webpack，会自动在当前目录中寻找 webpack 的可执行文件。

**注意**：在 Windows 系统中某些情况可能存在不稳定性，比如 安装 node 时路径有空格等。这时需要使用第一种方法。

```shell
$ npx webpack
```

webpack 在当前目录的 dist 目录中输出结果。此时应有转译后的 main.js 文件。

这是webpack自带的一个加载器：babel-loader实现的。

### 优化生成方式-build

在 package.json 文件中 设置 script脚本中的 build方法。 

该方法将清空dist目录，并重新运行webpack，重新生成dist目录和其中的文件；更加利于开发。

**package.json**

#### Mac/Linux

```json
{
   "script": {
+    "build": "rm -rf dist; webpack"
   }
}
```

#### Windows

```json
{
   "script": {
+    "build": "rm -rf dist && webpack"
   }
}
```

#### 使用命令

这样你以后可以用下面的方法取代npx命令。

```shell
$ yarn build
```
或
```shell
$ npm run build
```

### 用插件清理dist目录

你也可以不用build方法，而是安装一个清理插件**clean-webpack-plugin**。工作原理和build方法相似。

#### 安装插件：

```shell
$ yarn add clean-webpack-plugin --dev
```

#### 配置插件：

**webpack.config.js**

```js

+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/main.js'
    },
    plugins: [
+     new CleanWebpackPlugin(['dist'])
    ]
  };
```

这样在你每次运行 **npx webpack** 时，只会重新生成新的文件而看不到旧的文件。

**注意：最好使用配置script脚本的方法来使用build实现这个功能，而不推荐使用插件。这是为了下面自定义不同模式的配置文件做基础。**

## 配置 configuration

这时候警告提示需要设置配置文件中的转译模式。需要对配置文件进行配置。

在操作之前，最好把它整成一个 git，每一步都 git。

**注意：以下每一步都是在上一步的操作基础上进行操作，有些相同相似的或不需要的代码则没有展示。请忽略代码中的 “+”（加号表示局部增加或替换代码）和相关注释说明代码。**

如果你有另外的需求，最后参阅原始文档进行配置。

### 路径及模式
在根目录下创建配置文件。

```shell
$ touch webpack.config.js
```

进入配置文件，写入以下代码。

**webpack.config.js**

```js
const path = require("path");

module.exports = {
    // 转译模式： development 开发者模式 / production 产品模式
  mode: "development",
    // 输入源
  entry: "./src/index.js",
    // 输出
  output: {
      // 文件名
    filename: "main.js",
      // 输出路径
    path: path.resolve(__dirname, "dist")
  }
};
```

### 缓存需求文件名

可以设置为生成 包含 MD5哈希 的字符串插入文件名用于 HTTP缓存。

**webpack.config.js**

```js
module.exports = {

   output: {
+    filename: "main.[contenthash].js"
   }

};
```

### 自动生成HTML

webpack 可以使用插件来自动生成 HTML。

在命令行安装插件：

```shell
$ yarn add html-webpack-plugin
```

安装后修改配置文件

**webpack.config.js**

```js
+ const HtmlWebpackPlugin = require('html-webpack-plugin');
  module.exports = {
+   plugins: [
+     new HtmlWebpackPlugin({
       //html 标题
+       title: 'Output Management'
+     })
+   ],
  };
```
- 执行命令构建命令 **yarn build** 可以在dist目录中生成一个 index.html文件。

- 该文件会自动引入你项目中的外部文件

- 如果你在外部文件名中引入了 MD5哈希字符串，那么每次 build 的同时 html文件也会自动更新。

- 引入js会在body标签的最后面加入 script链接。

- 引入 css 可以配置为以 style标签 形式插入还是 link标签 插入。

#### 使用模板来生成html

你也可以使用项目中写好的 html模板 来让 webpack 生成新的 html 。

该模板要放在 src 目录下面的 assets 文件夹中。

```shell
$ mkdir src/assets
$ touch src/assets/index.html
```

在插件的 html 配置中再加入一行：

**webpack.config.js**

```js
  module.exports = {
    plugins: [
      new HtmlWebpackPlugin({
+       template: 'src/assets/index.html'
      })
    ],
  };
```
注意：如果你的模板存在但内容为空，webpack 只会生成一个只有引入外部文件链接的 空html文件。

模板中的内容会覆盖你在插件中的配置，比如说 title。

如果你想使用配置文件中的 title，请在模板中这样写：

```html
   <head>
+    <title><%= htmlWebpackPlugin.options.title %></title>
   </head>
```

### 引入和自动生成CSS

首先在src目录中创建css文件，并在index.js文件中用import方法引入。

```shell
$ touch src/default.css
$ echo "body{background: red;}" >> src/default.css
$ echo "import './default.css'" >> src/index.js
```

安装加载器

```shell
$ yarn add css-loader style-loader --dev
```

修改配置文件。

- css-loader的作用是如果发现任何以.css结尾的文件名，那么就处理文件并读到js文件中。
- style-loader的作用是把读到的内容变为html文件中的style标签并放入head标签中。

**webpack.config.js**

```js
module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'main.js',
      path: path.resolve(__dirname, 'dist')
    },
+   module: {
+     rules: [
+       {
+         test: /\.css$/,
+         use: [
+           'style-loader',
+           'css-loader'
+         ]
+       }
+     ]
+   }
  };

```

使用 build 方法后在 dist 目录的 index.html 文件中插入 style 标签。 

这是通过js文件来动态实现的，因此你需要在 dist 目录里创建一个 web服务器 才能看到效果。

### 分离抽成css文件

使用**mini-css-extract-plugin**来打包css文件，把css样式从js文件中提取到单独的css文件中。

```shell
$ yarn add mini-css-extract-plugin --dev
```

配置文件

```js
+  const MiniCssExtractPlugin = require('mini-css-extract-plugin')

   module.exports = {
     plugins: [
+      new MiniCssExtractPlugin({
+        // Options similar to the same options in webpackOptions.output
+        // all options are optional
+        filename: '[name].[contenthash].css',
+        chunkFilename: '[id].[contenthash].css',
+        ignoreOrder: false, // Enable to remove warnings about conflicting order
+      }),
     ],
     module: {
       rules: [
         {
           test: /\.css$/,
           use: [
+            {
+              loader: MiniCssExtractPlugin.loader,
+              options: {
+                // you can specify a publicPath here
+                // by default it uses publicPath in webpackOptions.output
+                publicPath: '../',
+                hmr: process.env.NODE_ENV === 'development',
+              },
+            },
             'css-loader',
           ],
         },
       ],
     },
   };
```

使用build方法后，将会在dist目录里重新生成一个带有MD5哈希字符串的main.css文件，并且在html文件中以link标签方式从外部引入。

**此方法会把所有通过import方式引入的CSS文件全部整合为一个文件，并且有根据引入顺序来排列内容。**

## 预览页面-快速开发

使用**webpack-dev-server**可以设置一个简单的web服务器，无需build即可预览dist文件目录下的文件，并实现实时重新加载。

### 安装

在命令行

```shell
$ yarn add webpack-dev-server --dev
```

### 配置文件

修改配置文件，告诉开发服务器在哪里查找文件：

**webpack.config.js**

```js
  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
+   devtool: 'inline-source-map',
+   devServer: {
+     contentBase: './dist'
+   }
  };
```

### 添加脚本命令

添加一个script脚本可以直接运行开发服务器。

**package.json**

```json
{
   "script": {
+    "start": "webpack-dev-server --open"
   }
}
```

"- -open" 表示自动打开浏览器，可以删除或保留。

只需运行

```shell
$ yarn start
```

或

```shell
$ npm run start
```

即可在 **localhost:8080** 创建服务。
