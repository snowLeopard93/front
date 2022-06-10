
### 一、eslint配置

#### 1、.eslintrc.js

```javascript
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: ["plugin:vue/essential", "eslint:recommended", "@vue/prettier"],
  parserOptions: {
    parser: "babel-eslint",
  },
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",
  },
};
```

### 二、引入`eslint-config-airbnb`

#### 1、安装依赖

```bash
npm i eslint-config-airbnb --save-dev
```

#### 2、`.eslintrc.js`引入

```javascript
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: ["plugin:vue/essential", "eslint:recommended", "airbnb"],
  parserOptions: {
    parser: "babel-eslint",
  },
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",
  },
};
```

#### 3、版本不兼容

`eslint-config-airbnb`安装版本改为`18.0.0`

```json
{
  "name": "vue2-study",
  "version": "0.1.0",
  "private": true,
  "devDependencies": {
    "eslint": "^6.7.2",
    "eslint-config-airbnb": "^18.0.0",
    "eslint-plugin-prettier": "^3.3.1",
    "eslint-plugin-vue": "^6.2.2"
  }
}
```

### 三、rules

[eslint rules](https://eslint.org/docs/user-guide/configuring/rules#configuring-rules)

#### 1、import/no-extraneous-dependencies

```
'webpack-merge' should be listed in the project's dependencies, not devDependencies
```

#### 2、quotes

```
Strings must use singlequote
```

#### 3、no-param-reassign

```
Assignment to property of function parameter 'articleItem'.
```

### 四、disable Rules

#### 1、单行注释

```
// eslint-disable-next-line no-param-reassign
```

**推荐：**

[eslint API](https://eslint.org/docs/user-guide/getting-started)

[airbnb/javascript](https://github.com/airbnb/javascript)