### 一、安装

#### 1、使用 vue-cli 创建

使用 `vue-cli` 创建时 `Unit Testing` 选项选择 `Jest`

#### 2、安装 sinon

```
npm install sinon --save-dev

// cnpm install sinon --save-dev
```

#### 3、安装 referee

```
npm install @sinonjs/referee --save-dev

// cnpm install @sinonjs/referee --save-dev
```

### 二、基本语法

#### 1、测试组件渲染出来的html

**counter.spec.js**

```javascript
import { mount } from "@vue/test-utils";
import Counter from "@/components/Counter.vue";

describe("Counter.vue", () => {
    it("renders counter html", () => {
        const wrapper = mount(Counter);
        expect(wrapper.html()).toMatchSnapshot();
    })
});
```

#### 2、测试 dom 事件

> **示例1：计数器示例**

**counter.spec.js**

```javascript
import { mount } from "@vue/test-utils";
import Counter from "@/components/Counter.vue";

describe("Counter.vue", () => {
    it('button click should increment the count', () => {
        const wrapper = mount(Counter);
        const button = wrapper.find('button');
        button.trigger('click');
        expect(wrapper.vm.count).toBe(1);
    })
});
```

> **示例2：鼠标点击示例**

**yesNoComponent.spec.js**

```javascript
import YesNoComponent from '@/components/YesNoComponent.vue'
import { mount } from '@vue/test-utils'
import sinon from 'sinon'
import referee from '@sinonjs/referee'

describe('Click event', () => {
    it('Click on yes button calls our method with argument "yes"', () => {
        const spy = sinon.spy();
        const assert = referee.assert;
        const wrapper = mount(YesNoComponent, {
            propsData: {
                callMe: spy
            }
        });
        wrapper.find('button.yes').trigger('click');

        // spy.should.have.been.calledWith('yes')
        assert(spy.calledWith('yes'));
    })
})
```

**YesNoComponent.vue**

```vue
<template>
    <div>
        <button class="yes" @click="callYes">Yes</button>
        <button class="no" @click="callNo">No</button>
    </div>
</template>

<script>
    export default {
        name: 'YesNoComponent',
        props: {
            callMe: {
                type: Function
            }
        },
        methods: {
            callYes() {
                this.callMe('yes')
            },
            callNo() {
                this.callMe('no')
            }
        }
    }
</script>
```

**完整示例戳：** [vue-test-demo](https://github.com/snowLeopard93/vue-demo/tree/master/vue-cli/vue-test-demo)

**参考：**

[Vue Test Utils](https://vue-test-utils.vuejs.org/zh/)

[vue-test-utils github地址](https://github.com/vuejs/vue-test-utils)

[sinon](https://sinonjs.org/)