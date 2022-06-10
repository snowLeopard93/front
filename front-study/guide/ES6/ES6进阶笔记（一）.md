

### 一、构造函数

在ES6之前，对象是用一种称为`构造函数`的特殊函数来定义对象和它们的特征。

#### 1、创建对象的三种方式

> 对象字面量

> new Object()

> 自定义构造函数

#### 2、注意

**（1）首字母要大写**

**（2）和`new`一起使用**

#### 3、new在执行时做的四件事

**（1）在内存中创建一个新的空对象**

**（2）让this指向这个新对象**

**（3）执行构造函数里面的代码，给这个新对象添加属性和方法**

**（4）返回这个新对象**

### 二、原型和原型链

#### 1、构造函数原型 prototype

（1）JavaScript规定，每一个构造函数都有一个`prototype`属性，指向另一个对象。

（2）原型是一个对象，也称`prototype`为原型对象。

#### 2、对象原型 __proto__

（1）对象都有一个`__proto__`属性，指向构造函数的`prototype`原型对象。

（2）`__proto__`对象原型和原型对象`prototype`是等价的。

#### 3、constructor构造函数

（1）`__proto__`对象原型和原型对象`prototype`都有一个`constructor`属性。

（2）`constructor`称为构造函数，因为它指回构造函数本身。

#### 4、JavaScript的成员查找规则

### 三、this

#### 1、函数内部的this指向

| **调用方式**     | **非严格模式下this指向**     | **严格模式下this指向** |
| ------------- |-------------|-------------|
| **普通函数调用** | window | undefined |
| **构造函数调用** | 实例对象 | 实例对象 |
| **对象方法调用** | 该方法所属对象 | 该方法所属对象 |
| **事件绑定方法** | 绑定事件对象 | 绑定事件对象 |
| **定时器函数** | window | window |
| **立即执行函数** | window | |

#### 2、严格模式下this指向

> **严格模式下，全局作用域函数中的this是undefined**

> **严格模式下，如果构造函数不加new调用，this会报错**
>
#### 3、改变this指向

> **apply方法**

**示例：**

```javascript
var a = {
    name: "haha"
};

function fn(v1, v2) {
    console.log(this);
    console.log(v1);
    console.log(v2);
}

fn.apply(a, ["aaa", "bbb"]);
```

> **call方法**

**示例：**

```javascript
var a = {
    name: "haha"
};

function fn(v1, v2) {
    console.log(this);
    console.log(v1 + v2);
}

fn.call(a, 1, 2);
```

> **bind方法**

**示例：**

```javascript
var a = {
    name: "haha"
};

function fn() {
    console.log(this);
}

fn.bind(a);
```

**推荐：**

[JavaScript构造函数、实例、原型对象以及原型链](https://juejin.im/post/6864168067182968839)

[你还没搞懂this？](https://github.com/ljianshu/Blog/issues/7)

**参考：**

[2019全新javaScript进阶面向对象ES6之《构造函数和原型》](https://www.bilibili.com/video/BV1Kt411w7MP?p=23)

[2019全新javaScript进阶面向对象ES6之《函数内部的this指向》](https://www.bilibili.com/video/BV1Kt411w7MP?p=53)
