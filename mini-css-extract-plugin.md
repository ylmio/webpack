**作用**：

此插件用于 **提取** CSS文件，将此css文件放入到**单独的(各自的)**的文件中。它为那些包含css的js文件创建一个css文件(太绕了)。它支持**按需加载** （On-Demand-Loading ）和 **SourceMaps**

**安装**（install）：

```bash
npm install --save-dev mini-css-extract-plugin
```

**用法** (usage)

​	配置

1.publicPath:

Type：`String|Function`,

Default: the `publicPath` in `webpackOptions.output`

或者为指定目标文件自定义的公共路径



2.`esModule`

Type: `Boolean` 

Default: `false`

默认情况下，mini-css-extract-plugin生成使用CommonJS模块语法的JS模块。 在某些情况下，使用ES模块是有益的，例如在[module concatenation](https://webpack.js.org/plugins/module-concatenation-plugin/) and [tree shaking](https://webpack.js.org/guides/tree-shaking/).的情况下。

你可以可以使用以下方式启用ES模块语法：

**webpack.config.js**

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
 
module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              esModule: true,//启用ES模块语法
            },
          },
          'css-loader',
        ],
      },
    ],
  },
};
```

小例子：

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
 
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      // 与webpackOptions.output的配置项很相似 
      // 所有的配置项都是可选择的
      filename: '[name].css',
      chunkFilename: '[id].css',
      ignoreOrder: false, // 删除顺序冲突的警告
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              // 你可以在此指定一个publicPath
              // 若不指定，它默认使用webpackOptions.output中配置的publicPath
              publicPath: '../',
              hmr: process.env.NODE_ENV === 'development',
            },
          },
          'css-loader',
        ],
      },
    ],
  },
};
```

例子，当publicPath的值是个函数时：动态路径

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].css',
      chunkFilename: '[id].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: (resourcePath, context) => {
                  console.log(resourcePath, context);
                // publicPath 是相对路径
                // e.g. for ./css/admin/main.css the publicPath will be ../../
                // while for ./css/main.css the publicPath will be ../
                return path.relative(path.dirname(resourcePath), context) + '/';
              },
            },
          },
          'css-loader',
        ],
      },
    ],
  },
};
```

**高级配置的例子**

此插件应仅在装载程序链中没有样式装载程序的生产版本上使用，尤其是当您要在开发中使用HMR(热更新)时。

这是在开发中同时包含HMR和将样式提取到用于生产构建的文件中的示例。

省略了loaders 配置选项，以使其更加清晰，从而适应您的需求

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const devMode = process.env.NODE_ENV !== 'production';
 
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      // Options similar to the same options in webpackOptions.output
      // both options are optional
      filename: devMode ? '[name].css' : '[name].[hash].css',
      chunkFilename: devMode ? '[id].css' : '[id].[hash].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              hmr: process.env.NODE_ENV === 'development',
            },
          },
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      },
    ],
  },
};
```

#### 热更新 Hot Module Reloading (HMR)

在开发环境下extract-mini-css-plugin插件支持热加载HMR。**使得应用在运行状态下，不重载刷新就能更新、增加、移除模块的机制**

用我的话来阐述，就是 **在应用程序的开发环境，方便开发人员在不刷新页面的情况下，就能修改代码，并且直观地在页面上看到变化的机制**。在进行热更新时 添加一段热更新代码 进入打包之后的代码中 每隔一段时间向 hot--发送请求监测数据是否发生了改变 变化之后会重新下载代码  重新编译 进行页面处理。

reloadAll是一个选项，仅当HMR无法正常工作时才应启用。

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      // Options similar to the same options in webpackOptions.output
      // both options are optional
      filename: '[name].css',
      chunkFilename: '[id].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              // 只在开发环境下配置使用热更新
              hmr: process.env.NODE_ENV === 'development',
              // 仅当HMR无法正常工作时才应启用.
              reloadAll: true,
            },
          },
          'css-loader',
        ],
      },
    ],
  },
};
```

**提取所有的css文件为一个单独的文件**

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        styles: {
          name: 'styles',
          test: /\.css$/,
          chunks: 'all',
          enforce: true,
        },
      },
    },
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
};
```

**根据 entry 提取 css文件**

你也可以根据webpack 的entry name 来提取css文件。

如果您动态导入路由，但希望根据entry将CSS捆绑在一起，则他的功能特别有用。这也防止了ExtractTextPlugin引起的CSS复制问题。

```js
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
 
function recursiveIssuer(m) {
  if (m.issuer) {
    return recursiveIssuer(m.issuer);
  } else if (m.name) {
    return m.name;
  } else {
    return false;
  }
}
 
module.exports = {
  entry: {
    foo: path.resolve(__dirname, 'src/foo'),
    bar: path.resolve(__dirname, 'src/bar'),
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        fooStyles: {
          name: 'foo',
          test: (m, c, entry = 'foo') =>
            m.constructor.name === 'CssModule' && recursiveIssuer(m) === entry,
          chunks: 'all',
          enforce: true,
        },
        barStyles: {
          name: 'bar',
          test: (m, c, entry = 'bar') =>
            m.constructor.name === 'CssModule' && recursiveIssuer(m) === entry,
          chunks: 'all',
          enforce: true,
        },
      },
    },
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
};
```

#### Module Filename Option 模块文件名配置

使用 `moduleFilename` 参数你可以使用 chunk data 自定义文件名。他在处理多入口文件时特别有用。在下面的示例中，我们将使用moduleFilename将生成的CSS输出到另一个目录中。

```js
const miniCssExtractPlugin = new MiniCssExtractPlugin({
  moduleFilename: ({ name }) => `${name.replace('/js/', '/css/')}.css`,
});
```

#### Long Term Caching长期缓存

对于长期缓存，请使用 `filename: "[contenthash].css"`。name名字都是可配置的。

### Remove Order Warnings 删除警告

对于通过使用范围界定或命名约定来缓解CSS排序的项目，可以通过将插件的ignoreOrder标志设置为true来禁用CSS排序警告。

```js
new MiniCssExtractPlugin({
  ignoreOrder: true,
}),
```

