### 一、webpack的安装

```
npm install webpack webpack-cli --save-dev
```

### 二、webpakc的基本使用

#### 1、webpack.config.js

**entry：** 打包入口；

**output：** 输出路径；

**mode：** 模式，可选值为`development`和`production`，使用不同的模式，会默认启用一些不同的plugin，如`UglifyJsPlugin`等。

**rules：** 配置规则，可以将 `webpack` 的 `loader` 写到里面；

```
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    mode: 'production',
    module: {
        rules: [
            // 加载css
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            // 加载图片
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            // 加载字体
            {
                test: /\.(woff|woff2|eot|ttf|otf)$/,
                use: [
                    'file-loader'
                ]
            }
        ]
    }
};
```

#### 2、常用命令

**（1）** config：指定不同的`webpack.config.js`

```
webpack --config webpack.dev.config.js
```

### 三、webpack常用loader

#### [file-loader](https://www.npmjs.com/package/file-loader)

用来加载文件，如图片等。

#### [url-loader](https://www.npmjs.com/package/url-loader)

用来加载文件，但当文件大小小于某个值时，会返回dataUrl。

#### [babel-loader](https://www.npmjs.com/package/babel-loader)

用来对JavaScript的一些语法进行处理。

#### [css-loader](https://www.npmjs.com/package/css-loader)

用来对css进行处理。

#### [style-loader](https://www.npmjs.com/package/style-loader)

用来对style进行处理。

#### [px2rem-loader](https://www.npmjs.com/package/px2rem-loader)

用来将css和style里的px转换成rem。

#### [raw-loader](https://www.npmjs.com/package/raw-loader)

用来将文件转换成字符串。

#### [image-webpack-loader](https://www.npmjs.com/package/image-webpack-loader)

用来将图片进行压缩。

**参考：**

[webpack中文文档](https://www.webpackjs.com/concepts/)

[webpack loader](https://www.webpackjs.com/loaders/)