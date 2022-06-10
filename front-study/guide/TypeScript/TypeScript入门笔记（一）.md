### 一、前置

#### 1、安装TypeScript

```
npm install -g typescript
```

#### 2、第一个TypeScript

**（1）第一个ts文件**

**示例：（greeter.ts）**

```typescript
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

**（2）编译ts文件**

```
tsc greeter.ts
```

### 二、基本语法

#### 1、类型注解

```typescript
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

#### 2、接口

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
#### 3、类

**示例：（greeter.ts）**
```typescript
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

**编译后：（greeter.js）**

```javascript
var Student = /** @class */ (function () {
    function Student(firstName, middleInitial, lastName) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
    return Student;
}());
function greeter(person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
var user = new Student("Jane", "M.", "User");
document.body.innerHTML = greeter(user);
```

**参考：**

[TypeScript 入门教程](https://ts.xcatliu.com/)

[TypeScript 中文手册](https://typescript.bootcss.com/)

[5分钟了解 TypeScript](https://typescript.bootcss.com/tutorials/typescript-in-5-minutes.html)

