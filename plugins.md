插件是 webpack 的支柱功能。

**插件的目的在于解决 loader 无法实现的其他事。**

 webpack 插件是一个具有 apply 属性的 JavaScript 对象。

**用法**：由于插件可以携带参数/选项，必须在webpack 配置中，向 `plugins` 属性传入 `new` 实例。

```javascript
//webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
const webpack = require('webpack'); //访问内置的插件
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```



