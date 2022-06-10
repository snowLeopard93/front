## Vue入门笔记（一）

本部分主要介绍两点内容，第一点是**基本概念**，第二点是**常用指令**。 

其中，**基本概念**包括Vue实例、指令和生命周期钩子。

### 一、基本概念

#### 1、Vue实例

```javascript
new Vue({
  el: '#app',
  data: {
    msg: 'Hello Vue'
  },
  methods: {
    say: function() {
        console.log("aaa");
    },
    write: function() {
        console.log("bbbb");
    }
  }
})
```

**完整示例戳：**[0.HelloVue.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/0.HelloVue.html)

#### 2、指令

`指令 (Directives)` 是带有 `v-` 前缀的特殊 `attribute`。指令 `attribute` 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，**当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM**。

#### 3、生命周期钩子

**初始化阶段：** `beforeCreate`、`created`、`beforeMount`、`mounted` 

**执行阶段：** `beforeUpdate`、`updated` 

**销毁阶段：** `beforeDestroy`、`destroyed`

![vue-lifecycle.png](../../images/Vue/vue-lifecycle.png)

**示例：**

```javascript
new Vue({
    el: '#app',
    data: {
       msg: "hello",
    },
    // 实例完全被创建出来之前执行它
    beforeCreate() {
    },
    created() {
    },
    // 模板在内存中编译完成，但未将其渲染到页面
    beforeMount() {
    },
    // 内存中模板已经挂载到页面，从创建阶段进入到运行阶段
    mounted() {
    },
    // 页面没有被更新，但数据已经被更新了
    beforeUpdate() {
    },
    // 页面被更新，与数据一致
    updated() {
    },
    // Vue实例从运行阶段进入到销毁阶段，但实例上的data、methods、过滤器、指令等等还处于可用状态
    beforeDestroy() {
    },
    // 实例被完全销毁，data、methods、过滤器、指令等等已经不可用了
    destroyed() {
    }
})
```

**完整示例戳：**[2.life-demo1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basic/2.life-demo1.html)

### 二、常用指令

#### 1、常用指令介绍

> **v-if、v-else、v-else-if**

**注意：** `v-else-if` 是2.1.0 新增。

`v-if` 用于条件性地渲染一块内容。`v-else`用于表示`v-if` 的`else`部分，`v-else-if`用于表示`v-if` 的`else-if`部分。

**示例：**

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

**完整示例戳：**[1.v-if.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/1.v-if.html)

> **v-show**

`v-show`用于根据条件展示元素。

**注意：**

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性**的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的**切换开销**，而 `v-show` 有更高的**初始渲染开销**。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

**完整示例戳：**[2.v-show.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/2.v-show.html)

> **v-for**

`v-for`基于一个数组来渲染一个列表。

**完整示例戳：**[3.v-for.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/3.v-for.html)

> **v-model**

用于在表单`<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。

**完整示例戳：**[4.v-model.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/4.v-model.html)

> **v-bind**

`v-bind`指令可以用于响应式地更新 HTML attribute。

**示例1：**

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

**示例2：v-bind:class**

```html
<div v-bind:class="{ active: isActive }"></div>

<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>

<div v-bind:class="[activeClass, errorClass]"></div>
```

**示例3：v-bind:style**

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

<div v-bind:style="[baseStyles, overridingStyles]"></div>

<!-- 2.3.0+ -->
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

**完整示例戳：**[5.v-bind.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/5.v-bind.html)

> **v-on**

`v-on`用于监听DOM事件。

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

**完整示例戳：**[6.v-on.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/6.v-on.html)

> **v-text、v-html**

`v-text`将数据解释为普通文本，`v-html`将数据解释为HTML代码。

**完整示例戳：**[7.v-html.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/basicDirectives/7.v-html.html)

> **v-cloak**

`v-cloak`用于解决插值表达式闪烁的问题。

#### 2、Vue 和 AngularJs 常用指令对照

| **作用**          | **Vue** |  **AngularJS**         |
| ------------- |-------------|------------- |
| **条件渲染** | v-if、v-else、v-else-if | ng-if |
| **显示隐藏** | v-show| ng-show |
| **遍历** | v-for | ng-repeat |
| **双向数据绑定** | v-model | ng-model |
| **响应式地更新HTML attribute** | v-bind | ng-bind |
| **切换class**| v-bind:class | ng-class |
| **切换样式** | v-bind:style | ng-style |
| **点击** | v-bind:click | ng-click |

**持续更新中**~~

**推荐：**

[Vue API-指令](https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4)

**参考：**

[Vue.js](https://cn.vuejs.org/)

