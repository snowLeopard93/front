## Vue进阶笔记（一）

本部分主要介绍两点内容，第一点是**过滤器**，第二点是**API参考**。 

### 一、过滤器

#### 1、全局过滤器

> **基本语法**

```javascript
Vue.filter('过滤器名称', function(data, arg, arg2){
    return data + arg + arg2;
})
```

**示例：**

```html
<div id="app">
   <div>{{msg | replaceName("不喜欢")}}</div>
</div>
<script>
    Vue.filter("replaceName", function(msg, arg){
        return msg.replace(/喜欢/g, arg);
    });
    new Vue({
        el: "#app",
        data: {
            msg: "我喜欢读书，喜欢听音乐，喜欢跑步，喜欢游泳"
        }
    })
</script>
```

**完整示例戳：**[0.globalFilter.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/advanced/0.globalFilter.html)

#### 2、私有过滤器

> **基本语法**

```javascript
new Vue({
    el: "#app",
    data: {
        msg: "111"
    },
    methods: {
        say: function() {
        
        }
    },
    filters: {
        filterName: function() {
        
        }
    }       
})
```

**示例：**

```html
<div id="app">
    <div>{{msg | replaceName("不喜欢")}}</div>
</div>
<script>
    new Vue({
        el: "#app",
        data: {
            msg: "我喜欢读书，喜欢听音乐，喜欢跑步，喜欢游泳"
        },
        filters: {
            replaceName: function(msg, arg) {
                return msg.replace(/喜欢/g, arg);
            }
        }
    })
</script>
```

**完整示例戳：**[1.privateFilter.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/advanced/1.privateFilter.html)

#### 3、注意

**（1）** 过滤器调用的时候，采取的是**就近原则**；

**（2）** 当存在同名的全局过滤器和私有过滤器时，**优先使用**私有过滤器。

### 二、API参考

#### 1、实例property

| **名称**          | **详细** |
| ------------- |-------------|
| **vm.$data** | Vue 实例观察的数据对象 |
| **vm.$props** | 当前组件接收到的 props 对象 |
| **vm.$el** | Vue 实例使用的根 DOM 元素 |
| **vm.$options** | 用于当前 Vue 实例的初始化选项 |
| **vm.$parent** | 父实例，如果当前实例有的话 |
| **vm.$root**| 当前组件树的根 Vue 实例，如果当前实例没有父实例，此实例将会是其自己 |
| **vm.$children** | 当前实例的直接子组件 |
| **vm.$slots** | 用来访问被插槽分发的内容 |
| **vm.$scopedSlots** | 用来访问作用域插槽 |
| **vm.$refs** | 一个对象，持有注册过 ref attribute 的所有 DOM 元素和组件实例 |
| **vm.$isServer**| 当前 Vue 实例是否运行于服务器 |
| **vm.$attrs** | 包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外) |
| **vm.$listeners** | 包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器 |

#### 2、实例方法/数据

| **名称**          | **详细** |
| ------------- |-------------|
| **vm.$watch( expOrFn, callback, [options] )** | 观察 Vue 实例上的一个表达式或者一个函数计算结果的变化 |
| **vm.$set( target, propertyName/index, value )** | 全局 Vue.set 的别名 |
| **vm.$delete( target, propertyName/index )** | 全局 Vue.delete 的别名 |

#### 3、实例方法/事件

| **名称**          | **详细** |
| ------------- |-------------|
| **vm.$on( event, callback )** | 监听当前实例上的自定义事件 |
| **vm.$once( event, callback )** | 监听一个自定义事件，但是只触发一次 |
| **vm.$off( [event, callback] )**| 移除自定义事件监听器 |
| **vm.$emit( eventName, […args] )** | 触发当前实例上的事件 |

#### 4、实例方法/生命周期

| **名称**          | **详细** |
| ------------- |-------------|
| **vm.$mount( [elementOrSelector] )** | 使用 vm.$mount() 手动地挂载一个未挂载的实例 |
| **vm.$forceUpdate()** | 迫使 Vue 实例重新渲染 |
| **vm.$nextTick( [callback] )**| 将回调延迟到下次 DOM 更新循环之后执行 |
| **vm.$destroy()** | 完全销毁一个实例 |

**参考：**

[Vue API](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B-property)