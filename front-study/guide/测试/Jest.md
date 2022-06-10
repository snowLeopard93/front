
### 一、安装

### 二、基础配置

### 三、常用语法

#### 1、it

```javascript
describe('Koa Compose', function () {
  it('should work', async () => {
    const arr = []
    const stack = []

    stack.push(async (context, next) => {
      arr.push(1)
      await wait(1)
      await next()
      await wait(1)
      arr.push(6)
    })

    stack.push(async (context, next) => {
      arr.push(2)
      await wait(1)
      await next()
      await wait(1)
      arr.push(5)
    })

    stack.push(async (context, next) => {
      arr.push(3)
      await wait(1)
      await next()
      await wait(1)
      arr.push(4)
    })

    await compose(stack)({})
    expect(arr).toEqual(expect.arrayContaining([1, 2, 3, 4, 5, 6]))
  })
```
#### 2、it.only

```javascript
describe('Koa Compose', function () {
    it.only('should be able to be called twice', () => {
    const stack = []

    stack.push(async (context, next) => {
      context.arr.push(1)
      await wait(1)
      await next()
      await wait(1)
      context.arr.push(6)
    })

    stack.push(async (context, next) => {
      context.arr.push(2)
      await wait(1)
      await next()
      await wait(1)
      context.arr.push(5)
    })

    stack.push(async (context, next) => {
      context.arr.push(3)
      await wait(1)
      await next()
      await wait(1)
      context.arr.push(4)
    })

    const fn = compose(stack)
    const ctx1 = { arr: [] }
    const ctx2 = { arr: [] }
    const out = [1, 2, 3, 4, 5, 6]

    return fn(ctx1).then(() => {
      assert.deepEqual(out, ctx1.arr)
      return fn(ctx2)
    }).then(() => {
      assert.deepEqual(out, ctx2.arr)
    })
  })
})
```

### 四、Matchers

#### 1、真假相关

##### toBe

##### toEqual

测试接收值与期望值是否相等。

```javascript
expect(ctx2).toEqual(ctx)

expect(arr).toEqual([1, 6, 4, 2, 3])
```

##### toBeNull

##### toBeDefined

##### toBeUndefined

##### toBeTruthy

##### toBeFalsy

#### 2、数值相关

##### toBeGreaterThan

##### toBeGreaterThanOrEqual

##### toBeLessThan

##### toBeLessThanOrEqual

#### 3、字符串相关

##### toMatch

#### 4、数组和枚举

##### （1）toContain

##### （2）arrayContaining

测试接收到的数组元素是否都包含在期望的数组里面。

**示例：**

```javascript
expect(arr).toEqual(expect.arrayContaining([1, 2, 3, 4, 5, 6]))
```

#### 5、异常

##### toThrow

测试某个操作是否会抛出异常。

**示例：**

```javascript
expect(() => compose()).toThrow(TypeError)
```

#### 6、对象相关

##### （1）toBeInstanceOf

测试某个对象是否是期望的类型。

**示例：**

```javascript
expect(e).toBeInstanceOf(Error)
```

#### 7、其他

[Jest expect](https://www.jestjs.cn/docs/expect)



**推荐：**

[Jest 官网](https://www.jestjs.cn/)

[Jest 中文文档](https://www.jestjs.cn/docs/getting-started)

[Jest API](https://www.jestjs.cn/docs/api)