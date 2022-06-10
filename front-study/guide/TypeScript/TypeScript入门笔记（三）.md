
### 一、接口

```typescript
interface Person {
    name: string;
    age: number;
}
```

#### 1、可选属性

```typescript
interface Person {
    name: string;
    age?: number;
}
```

#### 2、任意属性

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}
```

**注意：**

> 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集。

> 一个接口中只能定义一个任意属性。

#### 3、只读属性

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}
```

**注意：**

> 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候。（只能在对象刚刚创建的时候修改其值）

### 二、类

#### 1、访问修饰符

`TypeScript` 可以使用三种访问修饰符，分别是 `public`、`private` 和 `protected`。

> `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的。

> `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问。

**（1）** 使用 `private` 修饰的属性或方法，在子类中也是不允许访问的。

**（2）** 当构造函数修饰为 private 时，该类不允许被继承或者实例化。

> `protected` 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的。

**（1）** 当构造函数修饰为 protected 时，该类只允许被继承。

#### 2、 readonly修饰符

> 只读属性必须在声明时或构造函数里被初始化

> 如果 readonly 和其他访问修饰符同时存在的话，需要写在其后面。

#### 3、 abstract修饰符（抽象类）

`abstract`修饰符用于定义抽象类和其中的抽象方法。

**（1）** 抽象类做为其它派生类的基类使用，不会直接被实例化。不同于接口，抽象类可以包含成员的实现细节。

**（2）** 抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符。

### 三、函数

#### 1、函数声明

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```

#### 2、函数表达式

在 `TypeScript` 的类型定义中，=> 用来**表示函数的定义**，左边是输入类型，需要用括号括起来，右边是输出类型。在 ES6 中，=> 叫做箭头函数

```typescript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

#### 3、可选参数和剩余参数

> 可选参数

（1）与接口中的可选属性类似，我们用 ? 表示可选参数。

（2）可选参数必须跟在必须参数后面。

> 剩余参数

剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是你在省略号（ ...）后面给定的名字，你可以在函数体内使用这个数组。


**参考：**

[TypeScript 中文手册-接口](https://typescript.bootcss.com/interfaces.html)

[TypeScript 入门教程-接口](https://ts.xcatliu.com/basics/type-of-object-interfaces.html)

[TypeScript 中文手册-类](https://typescript.bootcss.com/classes.html)

[TypeScript 入门教程-类](https://ts.xcatliu.com/advanced/class.html)

[TypeScript 中文手册-函数](https://typescript.bootcss.com/functions.html)

[TypeScript 入门教程-函数的类型](https://ts.xcatliu.com/basics/type-of-function.html)