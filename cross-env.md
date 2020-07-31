### 是什么

运行跨平台设置和使用环境变量的脚本。

### 出现原因

当您使用NODE_ENV =production, 来设置环境变量时，大多数Windows命令提示将会阻塞(报错)。 （异常是Windows上的Bash，它使用本机Bash。）同样，Windows和POSIX命令如何使用环境变量也有区别。 使用POSIX，您可以使用：$ ENV_VAR和使用％ENV_VAR％的Windows。 
说人话：windows不支持NODE_ENV=development的设置方式。会报错。我们在自定义配置环境变量的时候，由于在不同的环境下，配置方式也是不同的。例如在window和linux下配置环境变量。

比如在window和Mac的环境变量的配置

```json
MAC:
"build": "NODE_ENV=production webpack --config webpack.config.js",
"dev": "NODE_ENV=development webpack-dev-server --config webpack.config.js"
```

```json
WINDOW:
"build": "set NODE_ENV=production webpack --config webpack.config.js",
"dev": "set NODE_ENV=development webpack-dev-server --config webpack.config.js"
```

```json
兼容方式:
"build": "cross-env NODE_ENV=production webpack --config webpack.config.js",
"dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"
```

### 解决

cross-env使得您可以使用单个命令，而不必担心为平台正确设置或使用环境变量。 只要在POSIX系统上运行就可以设置好，而cross-env将会正确地设置它。 
说人话: 这个迷你的包(cross-env)能够提供一个设置环境变量的scripts，让你能够以unix方式设置环境变量，然后在windows上也能兼容运行。

### 安装 (仅开发使用--save-dev)

```bash
// npm 安装：
npm install cross-env --save-dev
// yarn安装：
yarn add cross-env --dev
```

NODE_ENV = `development` / `production` 则是在打包时配置环境变量，用来区分`开发环境` / `正式环境`

### 使用

package.json

```json
"scripts": {
  "dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.dev.config.js",
  "build": "cross-env NODE_ENV=production webpack --config build/webpack.prod.config.js",
}
```

