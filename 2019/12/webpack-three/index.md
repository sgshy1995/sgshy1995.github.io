# Webpack高级：一键部署


## 前言

当你完成一个webpack项目时，就到了将代码搞到仓库里的时候。

- 你可以全部部署到github仓库，使用GithubPages域名的dist路径来浏览页面。

- 也可以将dist目录与根目录分开在不同的仓库。

- 当然是物尽其用，利用github分支功能分开部署。

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

比如仓库名称为 "webpack-demo-1"，你的 github用户名为 lisa。使用SSH方式。

```shell
$ git remote add origin git@github.com:lisa/webpack-demo-1.git
$ git push -u origin master
```

### 修改GithubPages路径

如果开启GithubPages，那么在master分支访问会得到404页面。因为你的网页预览目录应该在根目录下的dist目录。

那么你可以在首页的Edit选项中手动记录正确的访问地址，以方便预览网页。
在GithubPages提供的浏览地址后面加上："dist/index.html"。

## 分支部署

使用sh脚本文件和Github分支功能可以实现一键部署，并且把dist目录和项目其他文件分开。

### 添加ignore

回到上面的第三步。在提交代码前，把dist目录也加入。

**.gitignore**

```
/dist/
/node_modules/
```

### 提交代码

同样的方法，在本地提交代码。

### 上传至github

如果你新建了一个仓库，那么使用同样的方式上传。

如果已经全部部署到了一个github仓库，那么把本次本地提交上传至github。

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

将dist目录中的文件拷贝至当前目录（根目录）。

```shell
$ mv dist/* ./
```

将dist目录（已经为空）删除。

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

```shell
$ git push
$ git push --set-upstream origin gh-pages
```

### 使用分支预览

在GithubPages中选择gh-pages分支的域名来浏览网页即可。

## 用脚本实现这一切

### 回到master分支

```shell
$ git checkout master
```

### 创建脚本

```shell
$ touch deploy.sh
```

### 编写脚本

**deploy.sh**

```sh
yarn build &&
git checkout gh-pages &&
rm -rf src *.json *.js *.css *.html *.png *.jpg *.gif *.jpeg *.lock *.sh &&
mv dist/* ./ &&
rm -rf dist;
git add . &&
git commit . -m "update" &&
git push &&
git checkout -
```

### 注意

**注意：在使用脚本部署之前，一定要确认自己的本地master分支已经提交更新或上传至github仓库。以防止出错。**

### 一键部署

使用脚本文件一键部署，重复上述步骤并自动回到master分支。

```shell
$ sh deploy.sh
```
