

本部分主要介绍两点内容，第一点是**ES6的Module**，第二点是**AMD、CMD和CommonJS的介绍及比较**。

### 一、Module

#### 1、概述

**（1）** ES6 模块不是对象，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。

**（2）** ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。

**（3）** ES6模块的优点：

> **不再需要UMD模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。**

> **将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者navigator对象的属性。**

> **不再需要对象作为命名空间（比如`Math`对象），未来这些功能可以通过模块提供。**

**示例：**

```javascript
// ES6模块
import { stat, exists, readFile } from 'fs';
```

#### 2、严格模式

**（1）基本概念**

`ECMAScript 5`的**严格模式**是采用具有限制性JavaScript变体的一种方式，从而使代码显示地 脱离“马虎模式/稀松模式/懒散模式“（sloppy）模式。

**（2）严格模式的限制**

> **变量必须声明后再使用**

在正常模式下，如果一个变量没有声明就赋值，默认是全局变量。严格模式下，变量必须先声明再赋值。

> **函数的参数不能有同名属性，否则报错**

**示例：**

```javascript
function sum(a, a, c) { // !!! 语法错误
  "use strict";
  return a + a + c; // 代码运行到这里会出错
}
```

> **不能使用`with`语句**

**示例：**

```javascript
"use strict";
var x = 17;
with (obj) { // !!! 语法错误
  // 如果没有开启严格模式，with中的这个x会指向with上面的那个x，还是obj.x？
  // 如果不运行代码，我们无法知道，因此，这种代码让引擎无法进行优化，速度也就会变慢。
  x;
}
```

> **不能对只读属性赋值，否则报错**

**示例：**

```javascript
"use strict";
// 给只读属性赋值
var obj2 = { get x() { return 17; } };
obj2.x = 5; // 抛出TypeError错误
```

> **不能使用前缀 0 表示八进制数，否则报错**

> **不能删除不可删除的属性，否则报错**

**示例：**

```javascript
"use strict";
delete Object.prototype; // 抛出TypeError错误
```

> 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`

> **`eval`不会在它的外层作用域引入变量**

> **`eval`和`arguments`不能被重新赋值**

> **`arguments`不会自动反映函数参数的变化**

> **不能使用`arguments.callee`**

> **不能使用`arguments.caller`**

> **禁止`this`指向全局对象**

**注意：** ES6 模块之中，顶层的`this`指向`undefined`，即不应该在顶层代码使用`this`。

> **不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈**

> **增加了保留字（比如`protected`、`static`和`interface`）**

**（3）严格模式的使用**

> **为脚本开启严格模式**

**示例：**

```javascript
// 整个脚本都开启严格模式的语法
"use strict";
var v = "Hi!  I'm a strict mode script!";
```

> **为函数开启严格模式**

**示例：**

```javascript
function strict() {
  // 函数级别严格模式语法
  'use strict';
  function nested() { 
    return "And so am I!"; 
  }
  return "Hi!  I'm a strict mode function!  " + nested();
}

function notStrict() { 
  return "I'm not strict."; 
}
```

**（4）严格模式的优点**

> **在严格模式下通过`this`传递给一个函数的值不会被强制转换为一个对象。**

> **在严格模式中再也不能通过广泛实现的`ECMAScript`扩展“游走于”JavaScript的栈中。**

> **严格模式下的`arguments`不会再提供访问与调用这个函数相关的变量的途径。**

#### 3、export、import命令

模块功能主要由两个命令构成：`export`和`import`。`export`命令用于规定模块的对外接口，`import`命令用于输入其他模块提供的功能。

**（1）export**

> **输出变量**

**示例：**

```javascript
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
```

> **输出函数**

**示例：**

```javascript
export function multiply(x, y) {
  return x * y;
};
```

**（2）import**

**示例：**

```javascript
import { firstName, lastName, year } from './profile.js';
```

### 二、AMD、CMD、CommonJS

#### 1、AMD（Asynchronous Module Definition）

`AMD`采用**异步**方式加载模块，模块的加载不影响它后面语句的运行。

> **基本语法**

`require(['模块的名字']，callBack)`

**示例：**

```html
<script src="scripts/require.js"></script>
<script>
  require.config({
    baseUrl: "/another/path",
    paths: {
        "some": "some/v1.0"
    },
    shim: {
        'jquery.colorize': {
            deps: ['jquery'],
            exports: 'jQuery.fn.colorize'
        },
        'jquery.scroll': {
            deps: ['jquery'],
            exports: 'jQuery.fn.scroll'
        },
        'backbone.layoutmanager': {
            deps: ['backbone']
            exports: 'Backbone.LayoutManager'
        }
    },
    waitSeconds: 15
  });
  require( ["some/module", "my/module", "a.js", "b.js"],
    function(someModule,    myModule) {
        //This function will be called when all the dependencies
        //listed above are loaded. Note that this function could
        //be called before the page is loaded.
        //This callback is optional.
    }
  );
</script>
```

#### 2、CMD（Common Module Definition）

**示例：**

```javascript
/** AMD写法 **/
define(["a", "b", "c"], function(a, b, c) { 
    a.doSomething();
    if (false) {
        b.doSomething()
    } 
});

/** CMD写法 **/
define(function(require, exports, module) {
    var a = require('./a');
    a.doSomething();
    if (false) {
        var b = require('./b');
        b.doSomething();
    }
});
```

#### 3、CommonJS

> **CommonJS模块的特点**

（1）所有代码都运行在**模块作用域**，不会污染全局作用域。

（2）模块可以**多次加载**，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。

（3）模块加载的顺序，按照其在代码中出现的顺序。

> **module对象**

|  **属性**    | **描述**     |
| ------------- |-------------|
| **module.id** | 模块的识别符，通常是带有绝对路径的模块文件名。 |
| **module.filename** | 模块的文件名，带有绝对路径。 |
| **module.loaded** | 返回一个布尔值，表示模块是否已经完成加载。 |
| **module.parent** | 返回一个对象，表示调用该模块的模块。 |
| **module.children** | 返回一个数组，表示该模块要用到的其他模块。 |
| **module.exports** | 表示模块对外输出的值。 |

> **基本语法**

**（1）** `exports`

```javascript
var invisible = function () {
  console.log("invisible");
};

exports.message = "hi";

exports.say = function () {
  console.log(message);
}
```

**（2）** `require`

```javascript
var example = require('./example.js');
example
// {
//   message: "hi",
//   say: [Function]
// }
```

#### 4、比较

> **AMD、CMD、CommonJS比较**

|     | **AMD**     |  **CMD**    | **CommonJS**     |
| ------------- |-------------| ------------- |-------------|
| **模块加载（同步、异步）** | 异步 | 异步 | 同步 |
| **加载模块的语句** | require | define | require |
| **加载顺序** | 依赖前置、提前执行（预加载） | 依赖就近、延迟执行（懒加载） | |
| **实践** | requireJs | SeaJS | NodeJS |

> **ES6、CommonJS比较**

|     | **ES6**     |  **CommonJS**    |
| ------------- |-------------| ------------- |
| **输出** | 值的引用 | 值的拷贝 |
| **加载** | 编译时输出接口 | 运行时加载 |

**注意：**

`CommonJS` 加载的是一个对象（即`module.exports`属性），该对象只有在脚本运行完才会生成。而 `ES6` 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

**注：** 笔记中的示例代码来自于以下链接，后面会再对笔记中的示例代码进行补充完善和调整。

[ES6入门教程](https://es6.ruanyifeng.com/)

[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

[CommonJS规范](https://javascript.ruanyifeng.com/nodejs/module.html)

**推荐：**

[前端模块化——彻底搞懂AMD、CMD、ESM和CommonJS](https://www.cnblogs.com/chenwenhao/p/12153332.html#_label2)

**参考：**

[Module 的语法](https://es6.ruanyifeng.com/#docs/module)

[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

[CommonJS规范](https://javascript.ruanyifeng.com/nodejs/module.html)

[requireJs API](https://requirejs.org/docs/api.html)

[前端模块化—CommonJS、CMD、AMD、UMD和ESM](https://juejin.im/post/6857313590337077255)
