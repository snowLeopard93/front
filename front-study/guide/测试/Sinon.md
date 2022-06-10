### 一、常用方法

#### 1、spy

```javascript
import chai, { expect } from 'chai';
import { spy } from 'sinon';
import sinonChai from 'sinon-chai';
chai.use(sinonChai);
it('should accept an optional event handler map', () => {
     // chai 语法，判断传入一个空的Map，不会抛出异常
    expect(() => mitt(new Map())).not.to.throw;
    const map = new Map();
    const a = spy();
    const b = spy();
    map.set('foo', [a, b]);
    const events = mitt<{ foo: undefined }>(map);
    events.emit('foo');
    // sinon-chai 方法，判断只调用一次
    expect(a).to.have.been.calledOnce;
    expect(b).to.have.been.calledOnce;
});
```

**参考：**

[Sinon API](https://sinonjs.org/releases/v13/)

[sinon-chai](https://www.npmjs.com/package/sinon-chai)