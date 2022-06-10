### 一、插件plugin

#### 1、transform-remove-console

**（1）安装**

```
npm i babel-plugin-transform-remove-console --save
```

**（2）引入**

```babel.config.js
module.exports = {
  plugins: [
    'transform-remove-console'
  ]
};
```
**（3）作用**

代码打包时将`console`语句移除。

