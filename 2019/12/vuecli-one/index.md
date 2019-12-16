# Vue：安装 Vue-cli 与创建项目


## 前言

从 Webpack 到 [@Vue/Cli](https://cli.vuejs.org/zh/guide/)。

Vue-cli 是一个基于 Vue.js 进行快速开发的完整系统，提供了很多功能：

### CLI

CLI (@vue/cli) 是一个全局安装的 npm 包，提供了终端里的 vue 命令。

它可以通过 vue create 快速创建一个新项目的脚手架，或者直接通过 vue serve 构建新想法的原型。

你也可以通过 vue ui 通过一套图形化界面管理你的所有项目。

### CLI 服务

CLI 服务 (@vue/cli-service) 是一个开发环境依赖。它是一个 npm 包，局部安装在每个 @vue/cli 创建的项目中。

CLI 服务是构建于 webpack 和 webpack-dev-server 之上的。它包含了：

- 加载其它 CLI 插件的核心服务；
- **一个针对绝大部分应用优化过的内部的 webpack 配置；**
- 项目内部的 vue-cli-service 命令，提供 serve、build 和 inspect 命令。

## 安装

### 全局安装

使用 npm 或 yarn。

```bash
$ npm install -g @vue/cli
# 或
$ yarn global add @vue/cli 
```

### 查看版本

如果安装成功，你可以看到版本信息，并检查你的版本是否正确。

```bash
$ vue --version
```

## 创建项目

进入即将创建项目的目录。

### 使用vue create

使用 Vue 创建项目。

```bash
$ vue create vue-demo
```

### 配置

你会被提示选取一个 preset。

你可以选默认的包含了基本的 Babel + ESLint 设置的 preset，也可以选“手动选择特性”来选取需要的特性。

这个默认的设置非常适合快速创建一个新项目的原型，而手动设置则提供了更多的选项，它们是面向生产的项目更加需要的。

这里以入门者的角度选择配置。(回车进行下一步，↑↓键将光标上下移动选择，空格键选择或取消勾选)

- **目前方法 present**: 
  * Manually select features

- **包含特征 features**: 
  * Bable
  * CSS Pre-processors
  * Linter / Formatter
  * Unit Testing

- **CSS 预处理器 CSS pre-processors**: 
  * Sass/SCSS (With dart-sass)

- **代码校验/格式化 Linter / Formatter**: 
  * ESLint with error prevention only

- **何时触发校验 additional lint features**: 
  * Lint and fix on commit

- **单元测试方案 unit testing solution**: 
  * Jest

- **如何设置配置文件 prefer placing ...**: 
  * In dedicated config files

- **是否把以上选项作为以后项目的默认配置 save this as ...**: 
  * N

### 本地预览

开启一个本地预览服务器来检查项目是否成功创建。

```bash
$ yarn serve
```

如果看到以下提示，证明你的项目创建成功了。

```bash
# App running at:
# - Local: http://localhost:8080/ (端口号可能不一样)
# - Network: http://192.168.31.107:8080/
```

当你访问本地地址时，应该可以看到一个带有 Vue logo 和 相关功能介绍的页面。

## 使用 codesandbox.io

你也可以使用 codesandbox.io 直接生成一个可以在线修改和预览的 vue 项目。

- 进入 [官方网站](https://codesandbox.io)。
- 点击右上角的 "Create Sandbox"。
- 创建 Vue (vue-cli)。
- 修改，保存。
- 选择 File > Export to ZIP，即可导出代码到 zip 压缩文件。
