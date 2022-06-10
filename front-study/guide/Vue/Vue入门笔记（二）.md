## Vue入门笔记（二）

本部分主要介绍两点内容，第一点是**修饰符**，第二点是**计算属性和侦听属性**。 

其中，**修饰符**包括**事件修饰符**、**按键修饰符**、**系统修饰键**和**表单修饰符**，**计算属性和侦听属性**包括**计算属性**、**侦听属性**和**自定义侦听器**。

### 一、修饰符

#### 1、事件修饰符

> **.stop：阻止冒泡**

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>
```

**完整示例戳：**[0.stop.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/modifier/0.stop.html)

> **.prevent：阻止默认事件**

```html
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
```

```html
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
```

```html
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
```

**完整示例戳：**[1.prevent.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/modifier/1.prevent.html)

> **.capture：添加事件监听器时使用事件捕获模式**

```html
<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>
```

**完整示例戳：**[2.capture.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/modifier/2.capture.html)

> **.self：只当事件在该元素本身触发时触发回调**

```html
<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

**完整示例戳：**[3.self.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/modifier/3.self.html)

> **.once：事件只触发一次**

**完整示例戳：**[4.once.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/modifier/4.once.html)

#### 2、按键修饰符

**（1）常用按键别名**

`.enter`/`.tab`/`.delete` (捕获“删除”和“退格”键)/`.esc`/`.space`/`.up`/`.down`/`.left`/`.right`

**示例：**

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

**（2）自定义按键修饰符**

```javascript
// 可以使用 `v-on:keyup.f2`
Vue.config.keyCodes.f2 = 113;
```


#### 3、系统修饰键

> **.exact**

**示例：**

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

> **鼠标按钮修饰符**

`.left`

`.right`

`.middle`


#### 4、表单修饰符

> **.lazy**

**示例：**

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

> **.number**

**示例：**

```html
<input v-model.number="age" type="number">
```

> **.trim**

**示例：**

```html
<input v-model.trim="msg">
```

### 二、计算属性和侦听属性

#### 1、计算属性

如果某个值是由其他的值经过计算或其他处理得到的，可以使用`计算属性（computed）`。

**示例1：不使用计算属性**

```html
<div id="app">
    <input type="text" v-model="valueA"/>
    <label>+</label>
    <input type="text" v-model="valueB"/>
    <button @click="sum()">确定</button>
    <label>sum is：{{sumValue}}</label>
</div>
```

```javascript
  new Vue({
        el: '#app',
        data: {
            valueA: 2,
            valueB: 3,
            sumValue: 5
        },
        methods: {
            sum: function() {
                this.sumValue = Number.parseFloat(this.valueA) + Number.parseFloat(this.valueB);
            }
        }
    })
```

**完整示例戳：**[0.computeAttr-demo1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basic/0.computeAttr-demo1.html)

**示例2：使用计算属性**

```html
<div id="app">
    <input type="text" v-model="valueA"/>
    <label>+</label>
    <input type="text" v-model="valueB"/>
    <label>sum is：{{sumValue}}</label>
</div>
```

```javascript
 new Vue({
        el: '#app',
        data: {
            valueA: 2,
            valueB: 3
        },
        computed: {
            sumValue: function() {
                return Number.parseFloat(this.valueA) + Number.parseFloat(this.valueB);
            }
        }
    })
```

**完整示例戳：**[0.computeAttr-demo2.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basic/0.computeAttr-demo2.html)

#### 2、侦听属性

如果需要对某一个值的变化进行监听，则可以是用`侦听属性（watch）`。

**示例：**

```html
<div id="app">
    <input type="text" v-model="valueA"/>
    <label>+</label>
    <input type="text" v-model="valueB"/>
    <label>sum is：{{sumValue}}</label>
</div>
```

```javascript
new Vue({
        el: '#app',
        data: {
            valueA: 2,
            valueB: 3,
            sumValue: 5
        },
        watch: {
            valueA: function(val) {
                this.sumValue = Number.parseFloat(val) + Number.parseFloat(this.valueB);
            },
            valueB: function(val) {
                this.sumValue = Number.parseFloat(this.valueA) + Number.parseFloat(val);
            }
        },
    })
```

**完整示例戳：**[1.watchAttr-demo1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basic/1.watchAttr-demo1.html)

#### 3、自定义侦听器

**待补充~~**

