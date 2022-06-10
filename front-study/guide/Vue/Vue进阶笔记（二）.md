## Vue进阶笔记（二）

### 一、指令

#### 1、基本概念

> **钩子函数**

一个指令定义对象可以提供如下几个**钩子函数** (均为可选)：

`bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

`inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

`update`：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。

> **钩子函数参数**

**指令钩子函数**会被传入以下参数：

`el`：指令所绑定的元素，可以用来直接操作 DOM。

`binding`：一个对象，包含以下 property：

（1）`name`：指令名，不包括 v- 前缀。

（2）`value`：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。

（3）`oldValue`：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。

（4）`expression`：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。

（5）`arg`：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。

（6）`modifiers`：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。

（7）`vnode`：Vue 编译生成的虚拟节点。移步 VNode API 来了解更多详情。

（8）`oldVnode`：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

#### 2、自定义全局指令

> **基本语法**

```javascript
Vue.directive("指令名称", {
    // 每当指令绑定到这个元素上的时候，会立即执行这个函数，只执行一次
    bind: function(el, binding) {
    
    },
    // 元素插入到 DOM 的时候，会执行这个函数，只执行一次
    inserted: function(el, binding) {
    
    },
    // VNode更新的时候，会执行这个函数，可能执行多次
    updated: function(el, binding) {
    
    }         
})
```

**示例1：**

```html
<div id="app">
    <label>
        名称：
        <input type="text" v-focus>
    </label>
</div>
<script>
    // 自定义一个指令，用来将光标放到某个 input 标签中
    Vue.directive("focus", {
        inserted: function(el) {
            el.focus();
        }
    });

    new Vue({
        el: "#app",
        data: {
            defaultCode: "默认编码"
        }
    })
</script>
```

**示例2：**

```html
<div id="app">
    <label>
        编码：
        <input type="text" v-model="defaultCode" v-color>
    </label>
</div>
<script>
    // 自定义一个指令，用来修改 input 标签里值的颜色
    Vue.directive('color', {
        bind: function(el) {
            el.style.color = "red";
        }
    });

    new Vue({
        el: "#app"
    })
</script>
```

**完整示例戳：**[2.globalDirective-demo1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/advanced/2.globalDirective-demo1.html)

**示例3：** 使用`binding`参数传值

```html
<div id="app">
    <label>
        编码：
        <input type="text" v-model="defaultCode" v-focus v-color="'pink'">
    </label>
</div>
<script>
    // 自定义一个指令，用来将光标放到某个 input 标签中
    Vue.directive("focus", {
        // 元素插入到 DOM 的时候，会执行这个函数，只执行一次
        inserted: function(el) {
            el.focus();
        }
    });
    // 自定义一个指令，用来修改 input 标签里值的颜色
    Vue.directive('color', {
        bind: function(el, binding) {
            el.style.color = binding.value;
        }
    });

    new Vue({
        el: "#app",
        data: {
            defaultCode: "默认编码"
        }
    })
</script>
```

**完整示例戳：**[3.globalDirective-demo2.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/advanced/3.globalDirective-demo2.html)

#### 3、定义私有指令

> **基本语法**

```javascript
 new Vue({
        el: "#app",
        directives: {
            "指令名称": {
                bind: function(el, binding) {
                    
               },
               inserted: function(el, binding) {
                    
               },
               updated: function(el, binding) {
                    
               }   
            }
        }
    })
```

**示例：**

```html
<div id="app">
    <label>
        编码：
        <input type="text" v-model="defaultCode" v-fontsize="'30'">
    </label>
</div>
<script>
    new Vue({
        el: "#app",
        data: {
            defaultCode: "默认编码"
        },
        directives: {
            "fontsize": {
                bind: function(el, binding) {
                    el.style.fontSize = binding.value + "px";
                }
            }
        }
    })
</script>
```

**完整示例戳：**[4.privateDirective.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/advanced/4.privateDirective.html)

#### 4、指令函数的缩写形式

**示例：**

```javascript
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value;
})
```

**参考：**

[Vue自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)