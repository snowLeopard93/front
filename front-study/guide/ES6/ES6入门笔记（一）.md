

本部分主要介绍四点内容，第一点是**常用数据类型**，第二点是**常用方法对照**，第三点是**方法比较**，第四点是**类型转换**。 

其中，**常用数据类型**主要包括**字符串**、**数值**、**数组**、**Symbol**、**Set**和**Map**等六种数据类型；**常用方法对照**包括**数组相关**、**字符串相关**和**数值相关**的常用方法对照和**Set、Map、数组**三者之间的比较；**方法比较**则是针对第二点列出的各种方法进行比较；第四点是**各种数据类型之间的转换操作**。

### 一、常用数据类型

#### **1、字符串**

> **includes()、startsWith()、endsWith()** 

`includes()` 返回布尔值，表示是否找到了参数字符串；`startsWith()` 返回布尔值，表示参数字符串是否在原字符串的头部；`endsWith()` 返回布尔值，表示参数字符串是否在原字符串的尾部。

> **padStart()、padEnd()**

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。

> **trimStart()、trimEnd()**

ES2019 对字符串实例新增了`trimStart()`和`trimEnd()`这两个方法。它们的行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。

#### **2、数值**

> **Number.isFinite()、Number.isNaN()**

ES6 在Number对象上，新提供了`Number.isFinite()`和`Number.isNaN()`两个方法。

`Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是Infinity。

`Number.isNaN()`用来检查一个值是否为NaN。

> **Number.parseInt()、Number.parseFloat()**

ES6 将全局方法`parseInt()`和`parseFloat()`移植到Number对象上面，行为完全保持不变。

> **Number.isInteger()**

`Number.isInteger()`用来判断一个数值是否为整数。

#### **3、数组**

> **扩展运算符...**

（1）复制数组；

（2）合并数组；

**示例：**

```javascript
var arr1 = ["1", "2", "3"];
var arr2 = ["4", "5", "6"];
var allArr = [...arr1, ...arr2]

// ["1", "2", "3", "4", "5", "6"]
```

（3）与解构赋值结合；

（4）字符串；

> **Array.from()**

> **Array.of()**

`Array.of()`用于将一组值，转换为数组。

> **find()、findIndex()**

`find()`用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

`findIndex()`用于找出第一个符合条件的数组成员，然后返回第一个符合条件的数组成员的位置。如果没有符合条件的成员，则返回-1。

> **fill()**

`fill()`使用给定值，填充一个数组。

> **entries()，keys() 和 values()**

> **includes()**

`Array.prototype.includes()`返回一个布尔值，表示某个数组是否包含给定的值。

> **flat()**

`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

**示例：**

```javascript
[1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
```

#### **4、Symbol**

> **基本语法**

`Symbol`函数的参数只是表示对当前 `Symbol` 值的描述，因此**相同参数**的`Symbol`函数的返回值是不相等的。

**示例：**

```javascript
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

s1 === s2; // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false
```

> **应用场景**：消除魔术字符串；为对象定义一些非私有的、但又希望只用于内部的方法。

**注：** 魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。

**示例1：**

```javascript
// 原先的写法：
// const shapeType = {
//   triangle: 'Triangle'
// };

// 新的写法：
const shapeType = {
  triangle: Symbol()
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

**示例2：**

```javascript
let size = Symbol('size');

class Collection {
  constructor() {
    this[size] = 0;
  }

  add(item) {
    this[this[size]] = item;
    this[size]++;
  }

  static sizeOf(instance) {
    return instance[size];
  }
}

let x = new Collection();
Collection.sizeOf(x); // 0

x.add('foo');
Collection.sizeOf(x); // 1

Object.keys(x); // ['0']
Object.getOwnPropertyNames(x); // ['0']
Object.getOwnPropertySymbols(x) // [Symbol(size)]
```

> **常用方法**

**（1）`Object.getOwnPropertySymbols()`**

 `Object.getOwnPropertySymbols()`用来获取所有 `Symbol` 属性名。

**（2）`Symbol.for()`**

`Symbol.for()`接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。

**示例1：**

```javascript
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

**示例2：**

```javascript
Symbol.for("bar") === Symbol.for("bar");  // true

Symbol("bar") === Symbol("bar");  // false
```

**（3）`Symbol.keyfor()`**

`Symbol.keyFor()`返回一个已登记的 Symbol 类型值的key。

**示例1：**

```javascript
let s1 = Symbol.for("foo");
Symbol.keyFor(s1); // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

#### **5、Set**

> **应用场景**

 **（1）数组去重、字符串去重**

**示例：**

```javascript
var array = [1, 2, 3, 4, 5, 5, 6];
[...new Set(array)]; // [1, 2, 3, 4, 5, 6]

[...new Set('ababbc')].join('') // “abc”
```

 **（2）实现数组的并集、交集和差集**

**示例：**

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

> **常用方法**

**（1）操作方法：**

`Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。

`Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。

`Set.prototype.has(value)`：返回一个布尔值，表示该值是否为Set的成员。

`Set.prototype.clear()`：清除所有成员，没有返回值。

**（2）遍历方法：**

`Set.prototype.keys()`：返回键名的遍历器

`Set.prototype.values()`：返回键值的遍历器

`Set.prototype.entries()`：返回键值对的遍历器

`Set.prototype.forEach()`：使用回调函数遍历每个成员

#### **6、Map**

> **基本语法**

**示例：**

```javascript
const map = new Map();

map
.set(1, 'aaa')
.set(1, 'bbb');

map.get(1) // "bbb"
```

**注：**

**（1）** 只有对**同一个对象的引用**，Map 结构才将其视为同一个键。

**（2）** Map 的键实际上是跟**内存地址**绑定的，只要内存地址不一样，就视为两个键。

> **常用方法**

**（1）属性和操作方法：**

`size`

`Map.prototype.set(key, value)`

`Map.prototype.get(key)`

`Map.prototype.has(key)`

`Map.prototype.delete(key)`

`Map.prototype.clear()`

**（2）遍历方法：**

`Map.prototype.keys()`：返回键名的遍历器。

`Map.prototype.values()`：返回键值的遍历器。

`Map.prototype.entries()`：返回所有成员的遍历器。

`Map.prototype.forEach()`：遍历 Map 的所有成员。


### 二、常用方法对照

#### 1. 数组相关

| **作用**          | **ES5及以前** |  **ES6（广义）**         | **underscore**          | **jQuery**         |  **其他** |
| ------------- |-------------|------------- |-------------|-------------|-------------|
| **遍历数组** | forEach()**（ES5）** | entries()、keys()、values() | _.each() | $.each() | |
| **判断数组中是否包含指定值** | indexOf() | includes() |  | |
| **查询数组中指定值的第一条数据** | | find() | _.findWhere() |  |
| **判断数组中是否存在符合条件的数据** | some()**（ES5）** | |  | |
| **查询数组中符合条件的数据** | filter()**（ES5）** |  |  _.where()、  _.filter()  | $.grep() | |
| **对数组进行排序** | sort() | sort() | _.sortBy() | | |
| **查找数组中的最大值** | |  | _.max() | | Math.max.apply() |
| **查找数组中的最小值** | | | _.min() | | Math.min.apply() |
| **数组拷贝** | | ...**（扩展运算符）** | _.extend() | $.extend() | |
| **判断是否是数组** | Array.isArray() |  | _.isArray() | $.isArray() | |
| **数组做并集** | concat() | ...**（扩展运算符）** | _.union() | $.merge() | |
| **数组去重** | |  | _.uniq() | | |
| **获取数组中指定值的下标** | indexOf() | findIndex() | _.indexOf() | | |
| **数组拉平** | | flat() | _.flatten() | | |
| **数组转对象** | | Object.fromEntries() | _.object() | | |

#### 2. 字符串相关

| **作用**     | **ES5及以前**     | **ES6（广义）**         | **underscore**          | **jQuery**         |
| ------------- |-------------|------------- |-------------|-------------|
| **去除字符串的前后空格** | trim()**（ES5）** | trimStart()、trimEnd() |  | $.trim() |
| **判断字符串中是否存在某个子串** | indexOf() | includes()、startsWith()、endsWith() | _.contains() | 
| **判断是否是字符串** | |  | _.isString() | 
| **补全字符串长度** | | padStart()、 padEnd() | | 

#### 4、数值相关

| **作用**     | **ES5及以前**     | **ES6（广义）**         | **underscore**          | **jQuery**         |
| ------------- |-------------|------------- |-------------|-------------|
| **判断是否是数值** | | | _.isNumber() | $.isNumeric() |
| **字符串转整数、浮点数** | parseInt、parseFloat | Number.parseInt()、Number.parseFloat() |  | |
| **判断是否是NaN** | | Number.isNaN() | _.isNaN() |  |

#### 5、Set、Map和数组的比较

|      | **Set**     | **Map**         | **数组**          |
| ------------- |-------------|------------- |-------------|
| **存放内容** | 值 | 键值对 |  值 |
| **是否重复** | 不重复 | 不重复 | 可以重复 |
| **添加** | set(key, value) | set(key, value)  | push()、unshift() | 
| **删除** | delete(value) | delete(key)  | splice() |
| **查询某个值** | has(value) | get(key)、has(key) | includes() |
| **清除** | clear() | clear() | |
| **获取keys** | keys() | keys() | keys() |
| **获取values** | values() | values() | values() |
| **获取entries** | entries() | entries() | entries() |
| **遍历** | forEach() | forEach() | forEach() |


### 三、方法比较

#### 1、数组相关

> **（1）判断数组中是否包含指定值**

`indexOf()`方法用于获取数组中指定值的下标，可以通过值是否等于-1判断数组中是否包含指定值；ES6中的`includes()`方法返回的是一个布尔值；（与字符串的`includes()`类似）；

**注：** `indexOf()`方法内部使用严格相等运算符（===）进行判断，这会导致对**NaN**的误判。

```javascript
[NaN].indexOf(NaN); // -1
[NaN].includes(NaN); // true
```

> **（2）判断数组中是否存在符合条件的数据、查询出数组中符合条件的数据**

ES5中的`some()`方法返回的是一个**布尔值**；ES5中的`filter()`方法返回的是一个数组（里面是所有满足条件的元素）；

**注：** ES5中的`some()`方法找到符合条件的数据之后就停止遍历；ES5中的`filter()`方法则会遍历所有数据；

**示例：**

```javascript
var arr = ["aa", "bb", "cc"];
var findFlag = arr.some(function(item){
    return item === "aa";
});
// true

var arr = ["aa", "bb", "cc"];
var obj = arr.filter(function(item){
    return item == "aa";
})
// ["aa"]
```

> **（3）数组合并**

```javascript
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

### 四、类型转换

> **注：为更加直观地显示各种数据类型之间的转换操作，特意将对象的转换也放在此处。**
 
#### **1、数组**

> **（1）数组转对象**

```javascript
// 方法一： ES6 
Object.fromEntries([
  ["key1", "key2"],
  ["aa", "bb"]
]);
// { key1: "aa", key2: "bb" }

// 方法二：underscore
_.object(["key1", "key2"], ["aa", "bb"]);
// { key1: "aa", key2: "bb" }
```

> **（2）数组转Map**

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

#### **2、对象**

> **（1）对象转数组**

```javascript
// 方法一： ES6 
const obj = { "key1": "aa", "key2": "bb" };
Object.entries(obj);
// [ ["key1", "aa"], ["key2", "bb"] ]

// 方法二：underscore
_.pairs({"key1": "aa", "key2": "bb"});
// [["key1", "aa"], ["key2", "bb"]]
```

> **（2）对象转Map**

```javascript
let obj = {"a":1, "b":2};
let map = new Map(Object.entries(obj));
```

#### **3、Map**

> **（1）Map转数组**

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

> **（2）Map转对象**

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

> **（3）Map转JSON**

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap);
// '{"yes":true,"no":false}'

function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

#### **4、JSON**

> **（1）JSON转Map**

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}');
// Map {'yes' => true, 'no' => false}

function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```

**注：** 笔记中的示例代码来自于[ES6入门教程](https://es6.ruanyifeng.com/)和[underscorejs](http://underscorejs.org/)，后面会再对笔记中的示例代码进行补充完善和调整。

**参考：**

[ES6入门教程](https://es6.ruanyifeng.com/)

[underscorejs](http://underscorejs.org/)

[jQuery API](https://jquery.cuishifeng.cn/)
