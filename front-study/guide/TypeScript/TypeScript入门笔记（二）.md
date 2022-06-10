JavaScript 的类型分为两种：**原始数据类型（Primitive data types）** 和 **对象类型（Object types）**。

原始数据类型包括：**布尔值、数值、字符串、null、undefined** 以及 ES6 中的新类型 Symbol。

### 一、原始数据类型

#### 1、布尔值

```typescript
let isDone: boolean = false;
```

**注意：**

> 在 TypeScript 中，boolean 是 JavaScript 中的**基本类型**，而 Boolean 是 JavaScript 中的**构造函数**。其他基本类型（除了 null 和 undefined）一样。

#### 2、数值

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

#### 3、字符串

```typescript
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

#### 4、空值

```typescript
let unusable: void = undefined;
```

**注意：**

> 某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 

#### 5、Null 和 Undefined

```typescript
let u: undefined = undefined;
let n: null = null;
```

**注意：**

> 与 void 的区别是，undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量。

### 二、其他类型

#### 1、任意值

> 任意值（Any）用来表示允许赋值为**任意类型**。

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

> 与`unknown`类型不同，类型变量`any`允许您访问任意属性，即使不存在也是如此。这些属性包括函数，并且TypeScript不会检查它们的存在或类型。

```typescript
let looselyTyped: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
// OK, toFixed exists (but the compiler doesn't check)
looselyTyped.toFixed();
```

#### 2、数组

```typescript
let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];
```

#### 3、元组 Tuple

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

#### 4、枚举

**示例：**

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

**编译后：**

```javascript
var Color;
(function (Color) {
    Color[Color["Red"] = 1] = "Red";
    Color[Color["Green"] = 2] = "Green";
    Color[Color["Blue"] = 3] = "Blue";
})(Color || (Color = {}));
var colorName = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

#### 5、Never

**注意：**

> `never`类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使`any`也不可以赋值给`never`。


### 三、类型推论/联合类型/类型断言

#### 1、类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（`Type Inference`）的规则推断出一个类型。

#### 2、联合类型

联合类型（`Union Types`）表示取值可以为多种类型中的一种。

#### 3、类型断言

类型断言（`Type Assertion`）可以用来手动指定一个值的类型。

**示例1：**

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

**示例2：**

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

> 用途：

（1）将一个联合类型断言为其中一个类型。

（2）将一个父类断言为更加具体的子类。

（3）将任何一个类型断言为 any。

（4）将 any 断言为一个具体的类型。

> 总结：

（1）联合类型可以被断言为其中一个类型。

（2）父类可以被断言为子类。

（3）任何类型都可以被断言为 any。

（4）any 可以被断言为任何类型。

**参考：**

[TypeScript 入门教程-基础](https://ts.xcatliu.com/basics/index.html)

[Basic-Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)

[TypeScript 中文手册-基础类型](https://typescript.bootcss.com/basic-types.html)

[TypeScript 入门教程-类型断言](https://ts.xcatliu.com/basics/type-assertion.html)
