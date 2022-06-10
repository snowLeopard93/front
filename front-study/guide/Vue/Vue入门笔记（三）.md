本部分主要介绍三点内容，第一点是**创建组件和组件注册**，第二点是**Prop基础**，第三点是**插槽slot**。

### 一、创建组件和组件注册

#### 1、创建组件

> **使用Vue.component()创建组件**

**示例：**

```html
<script>
    Vue.component('hello-vue', {
        template: '<div>Hello World，Hello Vue！</div>'
    });
    new Vue({
        el: '#app'
    })
</script>
```

> **使用template标签创建组件**

```html
<div id="app"></div>
<template id="tmpl">
    <div>Hello World，Hello Vue！</div>
</template>
<script>
    Vue.component("hello-vue", {
        template: "#tmpl"
    });
    new Vue({
        el: '#app'
    })
</script>
```

#### 2、组件注册

> **全局注册**

```javascript
Vue.component('my-component-name', {
  // ... 选项 ...
})
```

> **局部注册**

**（1）基本语法：**
```javascript
var ComponentA = { /* ... */ };
var ComponentB = { /* ... */ };
var ComponentC = { /* ... */ };

new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

**示例：**

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>
```

**完整示例戳：** [App.vue](https://github.com/snowLeopard93/vue-demo/blob/master/vue-cli/src/App.vue)


#### 3、组件使用

**注意：**

**（1）** `data`必须是一个函数；

**示例：**

```html
<div id="app">
    <button-counter></button-counter>
</div>
<script>
    Vue.component('button-counter', {
        data: function () {
            return {
                count: 0
            }
        },
        template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
    });
    new Vue({
        el: '#app'
    })
</script>
```

**完整示例戳：**[1.count.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/component/1.count.html)

### 二、组件间通信

#### 1、父组件向子组件传递参数

```javascript
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

**示例：**

```html
<div id="app">
    <component-a v-bind:parentmsg="msg"></component-a>
</div>
<script>
   new Vue({
       el: "#app",
       data: {
           msg: "这是父组件的标题"
       },
       methods: {},
       components: {
           componentA: {
               template: "<h3>我是一个子组件 -- {{ parentmsg }}</h3>",
               props: ["parentmsg"]
           }
       }
   })
</script>
```

**完整示例戳：** [3.prop-1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/component/3.prop-1.html)

#### 2、子组件向父组件传递数据

**示例：**

```html
<div id="app">
    <component-a v-on:func="show"></component-a>
</div>

<template id="tmpl">
    <button v-on:click="childClick">这是子组件，点击传递参数给父组件</button>
</template>
<script>
    var componentA = {
        template: "#tmpl",
        methods: {
            childClick: function() {
                this.$emit('func', "我是子组件");
            }
        }
    };

    new Vue({
        el: "#app",
        methods: {
            show(data1) {
                console.log("调用了父组件身上的方法-----" + data1);
            }
        },
        components: {
            componentA
        }
    })
</script>
```

**完整示例戳：** [4.prop-2.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/component/4.prop-2.html)

#### 3、父组件调用子组件中的方法

**ParentComponent.vue**

```vue
<template>
  <ChildComponent ref="childComponent" />
  <button @click="refreshChild()"></button>
</template>
<script>
  export default {
    name: "ParentComponent",
    methods: {
      refreshChild() {
        this.$refs.childComponent.refresh();
      }
    }
  }
</script>
```

**ChildComponent.vue**

```vue
<template>
  <div>
    This is a child component！
  </div>
</template>
<script>
  export default {
    name: "ChildComponent",
    methods: {
      refresh() {
        console.log("refresh child component");
      }
    }
  }
</script>
```

### 三、slot

#### 1、slot

> **具名插槽**

**示例：**

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

> **作用域插槽**

**示例：**

```html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

> `v-slot`

在 2.6.0 中，为具名插槽和作用域插槽引入了一个新的统一的语法 (即 `v-slot` 指令)。它取代了 `slot` 和 `slot-scope` 这两个目前已被废弃但未被移除且仍在文档中的 attribute。

**示例：**

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

**注：** 笔记中的示例代码来自于[插槽](https://cn.vuejs.org/v2/guide/components-slots.html)，后面会再对笔记中的示例代码进行补充完善和调整。

**参考：**

[插槽](https://cn.vuejs.org/v2/guide/components-slots.html)

