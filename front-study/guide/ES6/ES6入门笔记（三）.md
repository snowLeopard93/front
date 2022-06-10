

本部分主要介绍两点内容，第一点是**Iterator**，第二点是**for...of循环**。

其中，`Iterator`从**基本概念**、**默认Iterator接口**、**使用场景**和**字符串的Iterator接口**四个方面进行介绍；`for...of`循环从可以使用`for...of`的范围和**遍历语法的比较**两个方面进行介绍。

### 一、Iterator

#### 1、基本概念

> **定义**

遍历器（`Iterator`）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 `Iterator` 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

> **作用**

**（1）** 为各种数据结构，提供一个统一的、简便的访问接口；

**（2）** 使得数据结构的成员能够按某种次序排列；

**（3）** ES6 创造了一种新的遍历命令`for...of`循环，`Iterator` 接口主要供`for...of`消费。

> **遍历过程**

**（1）** 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

**（2）** 第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

**（3）** 第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

**（4）** 不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。

#### 2、默认Iterator接口

ES6 规定，默认的 `Iterator` 接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。

#### 3、使用场景

> **解构赋值**

对数组和 `Set` 结构进行解构赋值时，会默认调用`Symbol.iterator`方法。

**示例：**

```javascript
let set = new Set().add('a').add('b').add('c');

let [x,y] = set;
// x='a'; y='b'

let [first, ...rest] = set;
// first='a'; rest=['b','c'];
```

> **扩展运算符**

**示例：**

```javascript
// 例一
var str = 'hello';
[...str]; //  ['h','e','l','l','o']

// 例二
let arr = ['b', 'c'];
['a', ...arr, 'd']
// ['a', 'b', 'c', 'd']
```

#### 4、字符串的Iterator接口

**示例：**

```javascript
var someString = "hi";
typeof someString[Symbol.iterator];
// "function"

var iterator = someString[Symbol.iterator]();

iterator.next();  // { value: "h", done: false }
iterator.next(); // { value: "i", done: false }
iterator.next();  // { value: undefined, done: true }
```

### 二、for...of循环

`for...of`循环可以使用的范围包括数组、`Set` 和 `Map` 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、`Generator` 对象，以及字符串。

#### 1、数组

`for...in`循环读取键名，`for...of`循环读取键值。如果要通过`for...of`循环，获取数组的索引，可以借助数组实例的`entries`方法和`keys`方法

**示例1：**

```javascript
const arr = ['red', 'green', 'blue'];

// for of
for(let v of arr) {
  console.log(v); // red green blue
}

// forEach
arr.forEach(function (element, index) {
  console.log(element); // red green blue
  console.log(index);   // 0 1 2
});
```

**示例2：**

```javascript
var arr = ['a', 'b', 'c', 'd'];

for (let a in arr) {
  console.log(a); // 0 1 2 3
}

for (let a of arr) {
  console.log(a); // a b c d
}
```

#### 2、Set 和 Map 结构

**示例：**

```javascript
var engines = new Set(["Gecko", "Trident", "Webkit", "Webkit"]);
for (var e of engines) {
  console.log(e);
}
// Gecko
// Trident
// Webkit

var es6 = new Map();
es6.set("edition", 6);
es6.set("committee", "TC39");
es6.set("standard", "ECMA-262");
for (var [name, value] of es6) {
  console.log(name + ": " + value);
}
// edition: 6
// committee: TC39
// standard: ECMA-262
```

#### 3、类似数组的对象

**示例：**

```javascript
// 字符串
let str = "hello";

for (let s of str) {
  console.log(s); // h e l l o
}

// DOM NodeList对象
let paras = document.querySelectorAll("p");

for (let p of paras) {
  p.classList.add("test");
}

// arguments对象
function printArgs() {
  for (let x of arguments) {
    console.log(x);
  }
}
printArgs('a', 'b');
// 'a'
// 'b'
```

#### 4、对象

**示例：**

```javascript
let es6 = {
  edition: 6,
  committee: "TC39",
  standard: "ECMA-262"
};

for (let e in es6) {
  console.log(e);
}
// edition
// committee
// standard

for (let e of es6) {
  console.log(e);
}
// TypeError: es6[Symbol.iterator] is not a function
```

#### 5、遍历语法的比较

**（1）** `for`循环较麻烦；

**（2）** `forEach`无法中途退出循环；

**（3）** `for...in`可以遍历数组的键名；

**缺点：**

> 数组的键名是数字，但是`for...in`循环是以字符串作为键名“0”、“1”、“2”等等。

> `for...in`循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。

> 某些情况下，`for...in`循环会以任意顺序遍历键名。

**（4）** `for...of`的优点：

> 有着同for...in一样的简洁语法，但是没有for...in那些缺点。

> 不同于forEach方法，它可以与break、continue和return配合使用。

> 提供了遍历所有数据结构的统一操作接口。

**注：** 笔记中的示例代码来自于[ES6入门教程](https://es6.ruanyifeng.com/)，后面会再对笔记中的示例代码进行补充完善和调整。

**参考：**

[ES6入门教程](https://es6.ruanyifeng.com/)