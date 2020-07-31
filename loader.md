loader 用于对模块的源代码进行转换。

loader 可以使你在 `import` 或 "加载" 模块时预处理文件。

loader 类似于其他构建工具中 "任务(task)" ，并提供了处理前端构建步骤的强大方法。

loader 可以将文件从不同的语言（如TypeScript）转换为JavaScript。

loader 可以将内联图像转换为 data URL。

loader 甚至允许你直接在 JavaScript 模块中 `import` CSS文件。

...

**首先**，安装相对应的 loader :（例如使用 loader 告诉 webpack 加载 CSS 文件）

```bash
npm install --save-dev css-loader
```

**然后**，指示 webpack 对每个 `.css` 使用 `css-loader`

```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' }
    ]
  }
};
```

**有三种使用 loader 的方式**：

1. 配置（推荐）：在 webpack.config.js 文件中指定 loader
2. 内联：在每个 `import` 语句中显示指定 loader
3. CLI：通过 `shell` 命令中指定他们

**举例1**：在 webpack.config.js 中的 module.rules 中指定多个loader。这种方式很简明，有助于代码变得简洁，比较推荐。

```js
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                {loader: "style-loader"},
                {
                    loader: "css-loader",
                    options: {
                        modules:true
                    }
                }
            ]
        }
    ]
}
```

**举例2**：使用 `import` 语句指定 loader 。使用 `!` 将资源中的 loader 分开。

```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

选项可以传递查询参数，例如 `?key=value&foo=bar`，或者一个 JSON 对象，例如 `?{"key":"value","foo":"bar"}`。

**举例3**：通过 CLI 使用 loader:

```sh
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

这会对 `.jade` 文件使用 `jade-loader`，对 `.css` 文件使用 [`style-loader`](https://www.webpackjs.com/loaders/style-loader) 和 [`css-loader`](https://www.webpackjs.com/loaders/css-loader)。