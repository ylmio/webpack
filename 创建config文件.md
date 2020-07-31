创建并导出webpack.config.js文件：

```js
touch webpack.config.js
```

此时项目根目录下会生成一个 webpack.config.js 文件。

导出配置文件的方式有两种，一种是：

```js
//webpack.config.js
module.exports = {
    
};
```

另一种是：

```js
//webpack.config.js
const config = {
    
};

module.exports = config;
```

这两种方式都可以导出webpack.config.js中的配置文件