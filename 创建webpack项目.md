基本安装：

首先我们创建一个项目文件夹，并进入此文件夹：

```js
mkdir webpack-demo
cd webpack-demo
```

初始化npm

```js
npm init -y
```

"npm init -y" : -y的意思是 yes，在 init 的时候省去了敲回车的步骤。会生成一个 package.json文件。省去的步骤就是让你输入package.json中参数配置的内容，比如 "name"、"version"、"description"、"main"、"scripts"、"keywords"、"author"、"license"等配置参数；这些参数可以后期进行配置。

接着在**本地安装webpack**

```js
npm install --save-dev webpack
#或者指定版本
npm install --save-dev webpack@version
```

如果你使用 webpack v4+ 版本，你还需要安装 [CLI](https://webpack.docschina.org/api/cli/)

```js
npm install --save-dev webpack-cli
```

**webpack推荐本地安装，易于后期项目升级。**

全局安装webpack:

```js
npm install --global webpack
```

不推荐全局安装 webpack。因为这会锁定你项目中的webpack版本，使用不同版本的webpack，可能会导致构建失败。

至此，配有webpack构建工具的一个项目基本搭建完成，项目内包含的一个主要的package.json文件，大致内容如下：

```josn
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```

