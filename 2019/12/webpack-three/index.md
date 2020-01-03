# Webpack/Vue 实用：一键部署


## 前言

当你完成一个 Webpack/Vue 项目时，将代码搞到仓库。

- 你可以全部部署到 github 仓库，使用 Github Pages 域名的 dist 路径来浏览页面。

- 也可以将 dist 目录与根目录分开在不同的仓库。

- 当然是物尽其用，利用 github 分支功能分开部署。

- 步骤太繁琐？脚本一键部署！

## 全部部署

### 本地打包

首先进入项目根目录，本地打包。

```shell
$ yarn build
```

### 创建仓库

在本地创建仓库。

```shell
$ git init
```

### 创建ignore

创建ignore文件。

```shell
$ touch .gitignore
```

**.gitignore**

```
/node_modules/
```

### 提交代码

提交代码。

```shell
$ git add .
$ git commit
```

### 上传到github仓库

在Github创建一个新的仓库，将本地文件上传至仓库。

比如仓库名称为 "webpack-demo-1"，你的 github 用户名为 lisa。使用SSH方式。

```shell
$ git remote add origin git@github.com:lisa/webpack-demo-1.git
$ git push -u origin master
```

### 修改 Github Pages 路径

如果开启 Github Pages，那么在 master 分支访问会得到 404 页面。因为你的网页预览目录应该在根目录下的 dist 目录。

那么你可以在首页的 Edit 选项中手动记录正确的访问地址，以方便预览网页。
在 GithubPages 提供的浏览地址后面加上："dist/index.html"。

## 分支部署

使用 sh 脚本文件和 Github 分支功能可以实现一键部署，并且把 dist 目录和项目其他文件分开。

### 添加 ignore

回到上面的第三步。在提交代码前，把 dist 目录也加入。

**.gitignore**

```
/dist/
/node_modules/
```

### 提交代码

同样的方法，在本地提交代码。

### 上传至 github

如果你新建了一个仓库，那么使用同样的方式上传。

如果已经全部部署到了一个 github 仓库，那么把本次本地提交上传至 github。

```shell
$ git push
```

### 新建一个分支

新建一个仓库分支。

```shell
$ git branch gh-pages
```

### 进入分支

```shell
$ git checkout gh-pages
```

### 删除文件

删除所有“不需要”的文件，只留下：

- /node_modules/

- /dist/

- .gitignore

### 拷贝

将 dist 目录中的文件拷贝至当前目录（根目录）。

```shell
$ mv dist/* ./
```

将 dist 目录（已经为空）删除。

```shell
$ rm -rf dist
```

那么目前的目录已经是一个网站的根目录。

### 再次本地提交

```shell
$ git add .
$ git commit . -m "delete source code"
```

### 上传至github

```bash
$ git push
$ git push --set-upstream origin gh-pages
```

### 回到master分支

```bash
$ git checkout master
```

如果你直接在项目中使用一键部署，是不需要这么多步骤的。你只需要创建分支，而不需要单独进入分支进行操作。

### 使用分支预览

在 Github Pages 中选择 gh-pages 分支的域名来浏览网页即可。

## 用脚本实现这一切

不需要这么多步骤，都交给脚本！

### 创建脚本

```bash
$ touch deploy.sh
```

### 创建分支

```bash
$ git branch gh-pages
```

### 编写脚本

**deploy.sh**

```bash
yarn build &&
git checkout gh-pages &&
rm -rf src tests css js img public *.json *.ico *.js *.css *.html *.png *.jpg *.gif *.jpeg *.lock *.sh *.md &&
mv dist/* ./ &&
rm -rf dist;
git add . &&
git commit . -m "update" &&
git push --set-upstream origin gh-pages &&
git checkout -
```

### 使用脚本

```bash
$ sh deploy.sh
```

等待运行完毕。你的项目会自动打包，输出结果的 dist 目录中的文件会自动添加到 gh-pages 分支并且上传到 github 仓库；结束后自动回到 master 分支。

### 注意

- 在使用脚本部署之前，一定要确认自己的**本地 master 分支已经提交更新或上传至 github 仓库**。以防止出错。
- 你可以在脚本的第三步选择删除掉 dist 目录外的所有文件，而不是手动选择去删除哪些文件，这无疑是更高效和安全的。
- 这里只展示一些简单的脚本语言。更强大的功能需要更多的 bash shell 语言来实现。
- 对于 Vue 项目，如果使用 vue-cli 生成的项目，一般已经无需配置及初始化，在修改 [publicPath](https://cli.vuejs.org/zh/config/#publicpath) 后就可以直接一键部署。

### 一键部署

所以，你每次创建新的项目，打包时就变得非常简单了！

你无须再创建新的仓库，只需创建新的分支。使用脚本文件一键部署，重复上述步骤并自动回到master分支。

```bash
$ cd my-demo
$ git add .
$ git commit . -m "update"
$ git push
$ git branch gh-pages
$ sh deploy.sh
```

记得在你的 Github Pages 中选择 gh-pages 分支来作为预览。
