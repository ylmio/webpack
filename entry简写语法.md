entry入口，作用是指示 webpack 应该使用哪个模块，来作为构建内部依赖图的开始。

**单页面应用程序**：

```js
//webpack.config.js
const config = {
    entry:{
        main : "./src/index.js"
    }
};

module.exports = config;
```

简写语法：

```js
//webpack.config.js
const config = {
    entry:"./src/index.js"
};

module.exports = config;
```

当我们开发单页面应用的时候，只有一个入口文件，我们可以使用下面这种简写语法的形式定义出口 **entry**.

**多页面应用程序**：

```javascript
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
```