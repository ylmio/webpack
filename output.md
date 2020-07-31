

单个入库起点的打包：

```js
//webpack.config.js
const path = require('path');
module.exports = {
    entry: './app.js',
    output: {
        path: path.resolve('./output'),
        filename: 'output-file.js'
    }
};
```

多个入口起点的打包：TODO

