### 一、路由基本配置

#### 1、引入 `VueRouter`

```javascript
import VueRouter from "vue-router";
Vue.use(VueRouter);
```

**注：** 使用`vue-cli`快速创建的vue项目，配置路由的js已经自动生成，路径为`src/router/index.js`

#### 2、定义路由

**（1）定义父子路由**

```javascript
{
    path: "/form/step-form",
    name: "stepForm",
    children: [
        {
            path: "/form/step-form",
            redirect: "/form/step-form/info"
        },
        {
            path: "/form/step-form/info",
            name: "info"
        },
        {
            path: "/form/step-form/confirm",
            name: "confirm",
        },
        {
            path: "/form/step-form/result",
            name: "result"
        }
    ]
}
```

**（2）动态加载组件**

使用如下语法，其中`webpackChunkName`用于动态打包时使用。
 
```javascript
component: () =>
    import(
        /* webpackChunkName: "form" */ "../views/Forms/StepForm/Step3"
    )
}
``` 

**示例：**

```javascript
{
    path: "/form/step-form/result",
    name: "result",
    component: () =>
        import(
            /* webpackChunkName: "form" */ "../views/Forms/StepForm/Step3"
        )
}
```

**（3）静态加载组件**

```javascript
import NotFound from "../views/404";

const routes = [
    {
        path: "*",
        name: "404",
        // route level code-splitting
        // this generates a separate chunk (about.[hash].js) for this route
        // which is lazy-loaded when the route is visited.
        component: NotFound
    }
]
```

**（4）添加页面加载时的进度条效果**

进度条插件使用[NProgress](https://ricostacruz.com/nprogress/)：

```javascript
import NProgress from "nprogress";
import "nprogress/nprogress.css";

router.beforeEach((to, from, next) => {
  if(to.path !== from.path) {
    NProgress.start();
  }
  next();
});

router.afterEach(() => {
  NProgress.done();
});
```

#### 3、示例

**部分代码如下：**

```javascript
const routes = [
  {
    path: "/",
    component: () =>
      import(/* webpackChunkName: "layout" */ "../layouts/BasicLayout"),
    children: [
      {
        path: "/",
        redirect: "/dashboard/analysis"
      },
      {
        path: "/dashboard",
        name: "dashboard",
        component: { render: h => h("router-view") },
        children: [
          {
            path: "/dashboard/analysis",
            name: "analysis",
            component: () =>
              import(
                /* webpackChunkName: "analysis" */ "../views/Dashboard/Analysis"
              )
          }
        ]
      },
      {
        path: "/form",
        name: "form",
        component: { render: h => h("router-view") },
        children: [
          {
            path: "/form/basic-form",
            name: "basicForm",
            component: () =>
              import(/* webpackChunkName: "form" */ "../views/Forms/BasicForm")
          },
          {
            path: "/form/step-form",
            name: "stepForm",
            component: () =>
              import(
                /* webpackChunkName: "form" */ "../views/Forms/StepForm/index"
              ),
            children: [
              {
                path: "/form/step-form",
                redirect: "/form/step-form/info"
              },
              {
                path: "/form/step-form/info",
                name: "info",
                component: () =>
                  import(
                    /* webpackChunkName: "form" */ "../views/Forms/StepForm/Step1"
                  )
              },
              {
                path: "/form/step-form/confirm",
                name: "confirm",
                component: () =>
                  import(
                    /* webpackChunkName: "form" */ "../views/Forms/StepForm/Step2"
                  )
              },
              {
                path: "/form/step-form/result",
                name: "result",
                component: () =>
                  import(
                    /* webpackChunkName: "form" */ "../views/Forms/StepForm/Step3"
                  )
              }
            ]
          }
        ]
      }
    ]
  }
];

const router = new VueRouter({
  mode: "history",
  base: process.env.BASE_URL,
  routes
});
```

### 二、路由拆分

```javascript
// src/router/index.js

import Vue from "vue";
import VueRouter from "vue-router";
import NProgress from "nprogress";
import "nprogress/nprogress.css";
import NotFound from "../views/404";
import Forbidden from "../views/403";
import findLast from "lodash/findLast";
import { check, isLogin } from "../utils/auth";
import { notification } from "ant-design-vue";

import systemRoutes from "./system";
import orderManageRoutes from "./orderManage";
import workbenchRoutes from "./workbench";

Vue.use(VueRouter);

let defaultRoutes = [
  {
    path: "/403",
    name: "403",
    hideInMenu: true,
    component: Forbidden
  },
  {
    path: "*",
    name: "404",
    hideInMenu: true,
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: NotFound
  }
];

const router = new VueRouter({
  // mode: "history",
  base: process.env.BASE_URL,
  routes: [
    ...workbenchRoutes,
    ...systemRoutes,
    ...orderManageRoutes,
    ...defaultRoutes
  ]
});

router.beforeEach((to, from, next) => {
  if (to.path !== from.path) {
    NProgress.start();
  }
  const record = findLast(to.matched, record => record.meta.authority);
  if (record && !check(record.meta.authority)) {
    if (!isLogin() && to.path !== "/user/login") {
      next({
        path: "/user/login"
      });
    } else if (to.path !== "/403") {
      notification.error({
        message: "403",
        description: "你没有权限访问，请联系管理员！"
      });
      next({
        path: "/403"
      });
    }

    NProgress.done();
  }
  next();
});

router.afterEach(() => {
  NProgress.done();
});

export default router;
```

```javascript
// src/router/system/index.js

let systemRoutes = [
  {
    path: "/system",
    name: "system",
    meta: {
      icon: "setting",
      title: "系统管理",
      authority: ["admin"]
    },
    component: () =>
      import(/* webpackChunkName: "layout" */ "../../layouts/BasicLayout"),
    children: [
      {
        path: "/system/user",
        name: "user",
        meta: {
          title: "用户管理"
        },
        hideChildrenInMenu: true,
        component: () =>
          import(
            /* webpackChunkName: "system" */ "../../views/System/User/index"
          ),
        children: [
          {
            path: "/system/user",
            redirect: "/system/user/index"
          },
          {
            path: "/system/user/index",
            name: "user",
            component: () =>
              import(
                /* webpackChunkName: "system" */ "../../views/System/User/index"
              )
          }
        ]
      },
      {
        path: "/system/role",
        name: "role",
        meta: {
          title: "角色管理"
        },
        hideChildrenInMenu: true,
        component: () =>
          import(
            /* webpackChunkName: "system" */ "../../views/System/Role/index"
          ),
        children: [
          {
            path: "/system/role",
            redirect: "/system/role/index"
          },
          {
            path: "/system/role/index",
            name: "role",
            component: () =>
              import(
                /* webpackChunkName: "system" */ "../../views/System/Role/index"
              )
          }
        ]
      },
      {
        path: "/system/notice",
        name: "notice",
        meta: {
          title: "公告管理"
        },
        hideChildrenInMenu: true,
        component: () =>
          import(
            /* webpackChunkName: "system" */ "../../views/System/Notice/index"
          ),
        children: [
          {
            path: "/system/notice",
            redirect: "/system/notice/index"
          },
          {
            path: "/system/notice/index",
            name: "notice",
            component: () =>
              import(
                /* webpackChunkName: "system" */ "../../views/System/Notice/index"
              )
          }
        ]
      },
      {
        path: "/system/log",
        name: "log",
        meta: {
          title: "日志管理"
        },
        hideChildrenInMenu: true,
        component: () =>
          import(
            /* webpackChunkName: "system" */ "../../views/System/Log/index"
          ),
        children: [
          {
            path: "/system/log",
            redirect: "/system/log/index"
          },
          {
            path: "/system/log/index",
            name: "log",
            component: () =>
              import(
                /* webpackChunkName: "system" */ "../../views/System/Log/index"
              )
          }
        ]
      }
    ]
  }
];

export default systemRoutes;
```