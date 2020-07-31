1. 在项目根目录下创建一个build目录；

2. 将webpack.dev.js、webpack.prod.js、webpack.common.js放到build目录中；

3. 修改package.json文件中的script标签，将config后的路径做修改：

   ```js
    "scripts": {
       "dev": "webpack-dev-server --config ./build/webpack.dev.js",
       "build": "webpack --config ./build/webpack.prod.js"
     },
   ```

4. 删除根目录下的 `webpack.config.js` 文件。那么我们可以开发环境下运行 `npm run dev` ，在生产环境下运行 `npm run build` 即可。

在实际的项目开发过程中，各个环境的配置文件有很多相同的地方，我们可以通过安装webpack的第三方模块`npm install webpack-merge -D`  即webpack-merge 包，将共用的内容抽离出来，放在 webpack.common.js 文件中，在使用的时候通过 webpack-merge 将两个文件整合到一起，就可以精简文件内容了。

```js
//webpack.dev.js
const merge = require('webpack-merge');
const commonConfig = require('./webpack.common.js');
const devConfig = {
  ......
}
module.exports = merge(commonConfig,devConfig);
```

这里注意一下merge的两个参数（不要写反了） → `merge( <公共部分> , <独有部分> )`

参考：https://www.jianshu.com/p/1689fbd35b39
参考：https://www.jianshu.com/p/1331f93de507

