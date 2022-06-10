

本部分主要介绍五点内容，第一点是**对象**，第二点是**Promise对象**，第三点是**Class**，第四点是**函数**，第五点是**async函数**。

其中**函数**从**函数的定义方式**、**函数的调用方式**、**函数参数的默认值、rest参数、箭头函数**等几个方面进行介绍。

### 一、对象

#### 1、常用方法

> **Object.is()**

**注意：** `===` 与 `Object.is()`的区别：

```javascript
+0 === -0; //true
NaN === NaN; // false

Object.is(+0, -0);// false
Object.is(NaN, NaN); // true

```

> **Object.assign()**

`Object.assign()`用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

> **Object.values()，Object.entries()**

`Object.values()`返回一个数组，成员是参数对象自身的（不含继承的）所有 **可遍历（enumerable）** 属性的键值。

`Object.entries()`返回一个数组，成员是参数对象自身的（不含继承的）所有 **可遍历（enumerable）** 属性的键值对数组。

> **Object.fromEntries()**

`Object.fromEntries()`是`Object.entries()`的逆操作，用于将一个键值对数组转为对象。

#### 2、常用方法对照

| **作用**     | **ES5及以前**     | **ES6**         | **underscore**          | **jQuery**         |
| ------------- |-------------|------------- |-------------|-------------|
| **获取所有key值** | Object.keys()**（ES5）** |  | _.keys() |  |
| **获取所有value值** | | Object.values() | _.values() | |
| **判断对象是否为空** | | | _.isEmpty() | $.isEmptyObject() |
| **判断是否是对象** | | | _.isObject() | |
| **对象拷贝** | | Object.assign()、...**（扩展运算符）** |  _.extend() | $.extend() |
| **对象合并** | | Object.assign()、...**（扩展运算符）** | | |
| **对象转数组** | | Object.entries() | _.pairs() | |
| **判断是否是null** | | | _.isNull() |  |
| **判断是否是undefined** | | | _.isUndefined() |  |

#### 3、方法比较

> **对象拷贝**

**（1）** ES6中的`Object.assign()`方法是**浅拷贝**；

```javascript
// 对象拷贝
// 方法一：
let aClone = { ...a };
// 方法二：
let aClone = Object.assign({}, a);

// 对象合并
// 方法一：
let ab = { ...a, ...b };
// 方法二：
let ab = Object.assign({}, a, b);
```

**（2）** `$.extend()`方法支持**浅拷贝**和**深拷贝**；

```javascript
var originObj = {"child": {"value": "1", "label": "2"}, "name": "3"};
var targetObj = {};
// 浅拷贝
targetObj = $.extend(false, targetObj, originObj);

// 修改 targetObj 中 child 中 的 value 属性
targetObj.child.value = "4";

// 输出结果：
// originObj：
// {"child": {"value": "4", "label": "2"}, "name": "3"};

// targetObj：
// {"child": {"value": "4", "label": "2"}, "name": "3"};

// 深拷贝
targetObj = $.extend(true, targetObj, originObj);

// 修改 targetObj 中 child 中 的 value 属性
targetObj.child.value = "4";

// 输出结果：
// originObj：
// {"child": {"value": "1", "label": "2"}, "name": "3"};

// targetObj：
// {"child": {"value": "4", "label": "2"}, "name": "3"};
```

**（3）** `_.extend()`方法是**浅拷贝**；

```javascript
var originObj = {"child": {"value": "1", "label": "2"}, "name": "3"};
var targetObj = {};
// 浅拷贝
targetObj = $.extend(targetObj, originObj);

// 修改 targetObj 中 child 中 的 value 属性
targetObj.child.value = "4";

// 输出结果：
// originObj：
// {"child": {"value": "4", "label": "2"}, "name": "3"};

// targetObj：
// {"child": {"value": "4", "label": "2"}, "name": "3"};
```

> 对象转数组

**示例：**

```javascript
// 方法一： ES6 
const obj = { "key1": "aa", "key2": "bb" };
Object.entries(obj);
// [ ["key1", "aa"], ["key2", "bb"] ]

// 方法二：underscore
_.pairs({"key1": "aa", "key2": "bb"});
// [["key1", "aa"], ["key2", "bb"]]
```

### 二、Promise对象

#### 1、Promise对象的特点

**（1）对象状态不受外界影响。** `Promise对象`有三种状态：`pending`（进行中）、`fulfilled`（已成功）、`rejected`（已失败）。

**（2）一旦状态改变，就不会再变。**

#### 2、基本用法

**示例：**

```javascript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

#### 3、常用方法

> **（1）`then`方法**

`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数（可选）是`rejected`状态的回调函数。

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

**示例：**

```javascript
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
```

> **（2）`catch`方法**

`catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

`Promise` 对象的错误具有**“冒泡”性质**，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

**示例：**

```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```

> **（3）`finally`方法（ES2018）**

`finally()`方法用于指定不管 `Promise` 对象最后状态如何，都会执行的操作。

**示例：**
```javascript
promise
.finally(() => {
  // 语句
});

// 等同于
promise
.then(
  result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```

> **（4）`all`方法**

`all()`方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

**示例：**

```javascript
// 生成一个Promise对象的数组
const promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON('/post/' + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```

> **（5）`race`方法**

`race()`方法同样是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

**示例：**

```javascript
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
```

> **（6）`allSettled`方法**

`allSettled()`方法接受一组 `Promise` 实例作为参数，包装成一个新的 `Promise` 实例。只有等到所有这些参数实例都返回结果，不管是`fulfilled`还是`rejected`，包装实例才会结束。该方法由 ES2020 引入。

**注意：** 当不关心异步操作的结果，只关心这些操作有没有结束时，可以使用该方法。

**示例：**

```javascript
const resolved = Promise.resolve(42);
const rejected = Promise.reject(-1);

const allSettledPromise = Promise.allSettled([resolved, rejected]);

allSettledPromise.then(function (results) {
  console.log(results);
});
// [
//    { status: 'fulfilled', value: 42 },
//    { status: 'rejected', reason: -1 }
// ]
```

> **（7）`try`方法**

**示例：**

```javascript
Promise.try(() => database.users.get({id: userId}))
  .then(...)
  .catch(...)
```

### 三、Class

#### 1、constructor 方法

`constructor`方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。

**示例：**

```javascript
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

#### 2、取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

#### 3、属性表达式

类的属性名，可以采用表达式。

**示例：**

```javascript
let methodName = 'getArea';

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

#### 4、extends实现继承

**示例：**

```javascript
class Point {
}

class ColorPoint extends Point {
}
```

#### 5、super关键字

**示例：**

```javascript
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

> **注意：**

**（1）** 与 ES5 一样，类的所有实例共享一个**原型对象**。

**（2）** 类和模块的内部，默认就是**严格模式**，所以不需要使用use strict指定运行模式。

**（3）** 类不存在**变量提升**（hoist），这一点与 ES5 完全不同。

**（4）** `name`属性总是返回紧跟在`class`关键字后面的类名。

**示例：**

```javascript
class Point {}
Point.name // "Point"
```

**（5）** ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（`Parent.apply(this)`）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用`super`方法），然后再用子类的构造函数修改this。

**（6）** ES6 要求，子类的构造函数必须执行一次`super`函数。

### 四、函数

#### 1、函数的定义方式

> **命名函数**

```javascript
 function fun() {}
```

> **匿名函数**

```javascript
var fun = function() {}
```
> **new Function()**

```javascript
var fun = new Function('a', 'b', 'console.log(a + b)');
```

#### 2、函数的调用方式

> **普通函数**

```javascript
function fun() {};
fun();
```
> **对象的方法**

```javascript
var obj = {
    play: function() {}
};

obj.play();
```
> **构造函数**

```javascript
function Star() {}

new Star();
```
> **绑定事件函数**

```javascript
var btn = ...;
btn.onclick - function() {};

```

> **定时器函数**

```javascript
setInterval(function(){}, 5000);
```

> **立即执行函数**

```javascript
(function(){
    console.log("aaa");
})()
```

#### 3、函数参数的默认值、rest参数和箭头函数

> **函数参数的默认值** 

**（1）** 参数变量是默认声明的，所以不能用`let`或`const`再次声明。

**（2）** 利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

**示例：**
```javascript
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello'); // Hello World
log('Hello', 'China'); // Hello China
log('Hello', ''); // Hello
```

> **rest参数**

ES6 引入 `rest` 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。`rest` 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

**示例：**

```javascript
function add(...values) {
  let sum = 0;
  for (var val of values) {
    sum += val;
  }
  return sum;
}
add(2, 5, 3) // 10
```

> **箭头函数**

ES6 允许使用“箭头”（`=>`）定义函数。

**箭头函数有几个使用注意点：**

**（1）** 函数体内的this对象，就是**定义时所在的对象**，而不是使用时所在的对象。

**（2）** 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

**（3）** 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用`rest`参数代替。

**（4）** 不可以使用`yield`命令，因此箭头函数不能用作`Generator`函数。

**示例1：**

```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
var sum = (num1, num2) => { return num1 + num2; };

// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

**示例2：**

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
```

**注：**`setTimeout`的参数是一个箭头函数，这个箭头函数的定义生效是在foo函数生成时，而它的真正执行要等到 100 毫秒后。如果是普通函数，执行时this应该指向全局对象window，这时应该输出21。但是，箭头函数导致this总是指向函数定义生效时所在的对象（本例是{id: 42}），所以输出的是42。

**示例3：**

箭头函数可以让`setTimeout`里面的this，绑定定义时所在的作用域，而不是指向运行时所在的作用域。

```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```

上面代码中，Timer函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的this绑定定义时所在的作用域（即Timer函数），后者的this指向运行时所在的作用域（即全局对象）。所以，3100 毫秒之后，timer.s1被更新了 3 次，而timer.s2一次都没更新。

**箭头函数可以让this指向固定化，这种特性很有利于封装回调函数。**

### 五、async函数

#### 1、基本用法

`async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到`await`就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

**示例：**

```javascript
// 方法一：
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);

// 方法二：
async function timeout(ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
```

#### 2、使用形式

```javascript
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...);

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
```

#### 3、基本语法

> **返回 Promise 对象**

> **Promise 对象的状态变化**

> **await 命令**
>
> **错误处理**

#### 4、注意点

**（1）** await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。

**示例：**

```javascript
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法
async function myFunction() {
  await somethingThatReturnsAPromise()
  .catch(function (err) {
    console.log(err);
  });
}
```

**（2）** 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。

**示例：**

```javascript
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

**（3）** await命令只能用在async函数之中，如果用在普通函数，就会报错。

**示例：**

```javascript
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  // 报错
  docs.forEach(function (doc) {
    await db.post(doc);
  });
}
```

**（4）** `async` 函数可以保留运行堆栈。

**注：** 笔记中的示例代码来自于[ES6入门教程](https://es6.ruanyifeng.com/)，后面会再对笔记中的示例代码进行补充完善和调整。

**参考：**

[ES6入门教程](https://es6.ruanyifeng.com/)
