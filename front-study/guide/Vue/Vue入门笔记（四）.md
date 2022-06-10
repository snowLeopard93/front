## Vue入门笔记（四）

本部分主要介绍`vue-router`的相关内容，包括`router-view`，`router-link`，路由传参、路由嵌套和命名视图等。

### 一、vue-router的引入

#### 1、script标签引入

https://unpkg.com/vue-router/dist/vue-router.js

```html
<script src="../lib/vue.js"></script>
<script src="../lib/vue-router-3.4.3.js"></script>
```

#### 2、npm安装

```
npm install vue-router
```

### 二、路由基本使用

#### 1、声明组件的模板对象

```html
<script>
    var login = {
        template: "<h3>登录</h3>"
    };
    
    var register = {
        template: "<h3>注册</h3>"
    };
</script>
```

#### 2、声明 VueRouter对象

```html
<script>
    var login = {
        template: "<h3>登录</h3>"
    };

    var register = {
        template: "<h3>注册</h3>"
    };
    
    var routerObj = new VueRouter({
        routes: [
            {"path": "/", "redirect": "/login"},
            {"path": "/login", component: login},
            {"path": "/register", component: register}
        ]
    });
</script>
```

#### 3、创建一个vue实例

```html
<script>
    var login = {
        template: "<h3>登录</h3>"
    };

    var register = {
        template: "<h3>注册</h3>"
    };
    
    var routerObj = new VueRouter({
        routes: [
            {"path": "/", "redirect": "/login"},
            {"path": "/login", component: login},
            {"path": "/register", component: register}
        ]
    });
    
    var vm = new Vue({
        el: "#app",
        data: {},
        methods: {},
        router: routerObj
    });
</script>
```

#### 4、添加 div块

```html
<div id="app">
    <a href="#/login">登录</a>
    <a href="#/register">注册</a>
    <router-view></router-view>
</div>
<script>
    var login = {
        template: "<h3>登录</h3>"
    };

    var register = {
        template: "<h3>注册</h3>"
    };

    var routerObj = new VueRouter({
        routes: [
            {"path": "/", "redirect": "/login"},
            {"path": "/login", component: login},
            {"path": "/register", component: register}
        ]
    });

    var vm = new Vue({
        el: "#app",
        data: {},
        methods: {},
        router: routerObj
    });
</script>
```

#### 5、将 a 标签改为 router-link

```html
<div id="app">
    <router-link to="login">登录</router-link>
    <router-link to="register">注册</router-link>
    <router-view></router-view>
</div>
```

**完整示例戳：**

[0.vue-router-demo1.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/router/0.vue-router-demo1.html)

[1.vue-router-demo2.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/router/1.vue-router-demo2.html)

### 三、路由传参

#### 1、query

第一步，在html中添加参数

```html
<div id="app">
    <router-link to="login?id=10">登录</router-link>
    <router-link to="register">注册</router-link>
    <router-view></router-view>
</div>
```

第二步，在组件中通过`$route.query`获取参数

```html
<script>
    var login = {
        template: "<h3>登录--{{ msg }}</h3>",
        data() {
            return {
                msg: ""
            }
        },
        created() {
            this.msg = this.$route.query.id;
        }
    };
</script>
```

**完整示例戳：** [2.vue-router-demo3.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/router/2.vue-router-demo3.html)

### 四、路由嵌套

#### 1、定义父路由

```html
<div id="app">
    <router-link to="/account">账户</router-link>
    <router-view></router-view>
</div>
```

#### 2、定义子路由

```html
<template id="tmpl">
    <div>
        <h1>这是account组件</h1>
        <router-link to="/account/login">登录</router-link>
        <router-link to="/account/register">注册</router-link>
        <router-view></router-view>
    </div>
</template>
```

#### 3、创建组件

```html
<script>
    var account = {
        template: "#tmpl"
    };

    var login = {
        template: "<h3>登录</h3>"
    };

    var register = {
        template: "<h3>注册</h3>"
    };

    var routerObj = new VueRouter({
        routes: [
            {"path": "/account", component: account,
                "children": [
                    {"path": "login", component: login},
                    {"path": "register", component: register}
                ]}
        ]
    });

    var vm = new Vue({
        el: "#app",
        data: {},
        methods: {},
        router: routerObj
    });
</script>
```

**完整示例戳：** [3.vue-router-demo4.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/router/3.vue-router-demo4.html)

### 五、命名视图

**示例：**

```html
<div id="app">
    <router-view></router-view>
    <div class="container">
        <router-view name="left"></router-view>
        <router-view name="main"></router-view>
    </div>
</div>

<script>
    var header = {
        template: "<h1 class='header'>这是头部区域</h1>"
    };

    var leftBox = {
        template: "<h1 class='left'>这是侧边栏区域</h1>"
    };

    var mainBox = {
        template: "<h1 class='main'>这是内容区域</h1>"
    };

    var routerObj = new VueRouter({
        routes: [
            {"path": "/", components: {
                "default": header,
                "left": leftBox,
                "main": mainBox
            }}
        ]
    });

    var vm = new Vue({
        el: "#app",
        data: {},
        methods: {},
        router: routerObj
    });
</script>
```

**完整示例戳：** [4.vue-router-demo5.html](https://github.com/snowLeopard93/vue-demo/blob/master/vue/router/4.vue-router-demo5.html)

**参考：**

[vue-router](https://router.vuejs.org/zh/)