### 一、基本语法

#### 1、.to.be.a/.to.be.an

判断数据类型

```javascript
describe('mitt', () => {
    it('should default export be a funcion', () => {
        expect(mitt).to.be.a('function');
    });
})
```

#### 2、.not.to.throw

判断不抛出异常

```javascript
describe('mitt', () => {
    it('should accept an optional event handler map', () => {
        expect(() => mitt(new Map())).not.to.throw;
    });
})
```

#### 3、.to.have.property

判断是否有某个属性

```javascript
describe('properties', () => {
    it('should expose the event handler map', () => {
        expect(inst)
            .to.have.property('all')
            .that.is.a('map')
        })
    })
})
```

#### 4、.is.a

判断是否是某一类型

```javascript
describe('properties', () => {
    it('should expose the event handler map', () => {
        expect(inst)
            .to.have.property('all')
            .that.is.a('map')
        })
    })
})
```

#### 5、.to.deep.equal

判断值是否相等

```javascript
it('should register handler for new type', () => {
    const foo = () => {};
    inst.on('foo', foo);
    expect(events.get('foo')).to.deep.equal([foo]);
})
```

##### 6、.to.be.empty

判断是否为空

```javascript
describe('off()', () => {
    it('should remove handlers for type', () => {
        const foo = () => {};
        inst.on('foo', foo);
        inst.off('foo', foo);
        expect(events.get('foo')).to.be.empty;
    })
})
```

##### 7、.to.have.lengthOf/.to.have.length

判断长度

```javascript
describe('off()', () => {
    it('should NOT normalize case', () => {
        const foo = () => {};
        inst.on('baz:bat!', foo);
        inst.off('baz:baT!', foo);
        expect(events.get('baz:bat!')).to.have.lengthOf(1);
    })
})
```

```javascript
describe('off()', () => {
    it('off("type") should remove all hanlders of the given type', () => {
        const foo = () => {};
        inst.on('bar', foo);
        expect(events.get('bar')).to.have.length(1);
    })
})
```

##### 8、.to.have.been.calledOnce.and

```javascript
describe('emit()', () => {
    it('should NOT ignore case', () => {
        const onFoo = spy(),
            onFOO = spy();
        events.set('Foo', [onFoo]);
        events.set('FOO', [onFOO]);

        inst.emit('Foo', 'Foo arg');
        inst.emit('FOO', 'FOO arg');
        expect(onFoo).to.have.been.calledOnce.and.calledWidth('Foo arg');
        expect(onFOO).to.have.been.calledOnce.and.calledWidth('FOO arg');
    })
})
```

**参考：**

[chaijs API](https://www.chaijs.com/api/)