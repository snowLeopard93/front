### 一、基本概念

#### 1、vuex

`Vuex` 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种**可预测**的方式发生变化。

#### 2、核心概念

> State

**（1）** `mapState` 辅助函数可以生成计算属性。

> Mutation

**（1）** 更改 `Vuex` 的 `store` 中的状态的唯一方法是提交 `mutation`。

**（2）** `mapMutations` 辅助函数将组件中的 `methods` 映射为 `store.commit` 调用。

**（3）** 在 `Vuex` 中，`mutation` 都是**同步事务**。

> Action

**（1）** `Action` 类似于 `mutation`，不同在于：`Action` 提交的是 `mutation`，而不是直接变更状态；`Action` 可以包含任意**异步操作**。

**（2）** `Action` 通过 `store.dispatch` 方法触发。

**（3）** `mapActions` 辅助函数将组件的 `methods` 映射为 `store.dispatch` 调用。

> Getter

**（1）** `Vuex` 允许我们在 store 中定义“getter”（可以认为是 `store` 的计算属性）。就像计算属性一样，`getter` 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

**（2）** `mapGetters` 辅助函数仅仅是将 `store` 中的 `getter` 映射到局部计算属性。

### 二、基本语法

```javascript
new Vuex.Store({
    // 值
    state: {
        
    },
    // 获取值
    getters: {
        
    },
    // 给异步请求使用
    actions: {
        incrementAsync() {
        
        }   
    },
    // 值改变时使用
    mutations: {
        increment() {
        
        }   
    }   
})
```

**示例1：计数器**

**store.js**

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

// root state object.
// each Vuex instance is just a single state tree.
const state = {
    count: 0
};

// mutations are operations that actually mutate the state.
// each mutation handler gets the entire state tree as the
// first argument, followed by additional payload arguments.
// mutations must be synchronous and can be recorded by plugins
// for debugging purposes.
const mutations = {
    increment (state) {
        state.count++
    },
    decrement (state) {
        state.count--
    }
};

// actions are functions that cause side effects and can involve
// asynchronous operations.
const actions = {
    increment: ({ commit }) => commit('increment'),
    decrement: ({ commit }) => commit('decrement'),
    incrementIfOdd ({ commit, state }) {
        if ((state.count + 1) % 2 === 0) {
            commit('increment')
        }
    },
    incrementAsync ({ commit }) {
        setTimeout(() => {
            commit('increment')
            // resolve()
        }, 1000)
    }
};

// getters are functions.
const getters = {
    evenOrOdd: state => state.count % 2 === 0 ? 'even' : 'odd'
};

// A Vuex instance is created by combining the state, mutations, actions,
// and getters.
export default new Vuex.Store({
    state,
    getters,
    actions,
    mutations
})
```

**main.js**

```javascript
import Vue from 'vue'
import store from './store/store'
import App from './App.vue'

Vue.config.productionTip = false;

new Vue({
  store,
  render: h => h(App),
}).$mount('#app')
```

**App.vue**

```vue
<template>
  <div id="app">
    Clicked: {{ $store.state.count }} times, count is {{ evenOrOdd }}.
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementIfOdd">Increment if odd</button>
    <button @click="incrementAsync">Increment async</button>
  </div>
</template>

<script>
  import { mapGetters, mapActions } from 'vuex'
  export default {
    computed: mapGetters([
      'evenOrOdd'
    ]),
    methods: mapActions([
      'increment',
      'decrement',
      'incrementIfOdd',
      'incrementAsync'
    ])
  }
</script>

<style>

</style>
```




**完整示例戳：**[vuex-counter-demo](https://github.com/snowLeopard93/vue-demo/tree/master/vuex/vuex-counter-demo)

**参考：**

[Vuex](https://vuex.vuejs.org/zh/)（[github地址](https://github.com/vuejs/vuex/tree/v3.5.1)）