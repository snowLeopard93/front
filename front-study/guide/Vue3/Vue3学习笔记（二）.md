
### 一、创建Vue3项目

#### 1、Vite + Vue3

```bash
npm init vite@latest my-vue-app --template vue
```

### 二、文件比对

#### 1、App.vue

**（1）Vue3**

```vue
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

**（2）Vue2.x**

```vue
<template>
  <div id="app">
  </div>
</template>
<script>
export default {
  data() {
    return {}
  },

}
</script>
<style>
</style>
```

#### 2、HelloWorld.vue

```vue
<script setup>
  import { ref } from 'vue'

  defineProps({
    msg: String
  })

  const count = ref(0)
</script>
<template>
  <h1>{{ msg }}</h1>
  <p>
    Recommended IDE setup:
    <a href="https://code.visualstudio.com/" target="_blank">VSCode</a>
    +
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
  </p>

  <p>
    <a href="https://vitejs.dev/guide/features.html" target="_blank">
      Vite Documentation
    </a>

    <a href="https://v3.vuejs.org/" target="_blank">Vue 3 Documentation</a>
  </p>

  <button type="button" @click="count++">count is: {{ count }}</button>
  <p>
    Edit
    <code>components/HelloWorld.vue</code> to test host module replacement.
  </p>
</template>
<style scoped>
a {
  color: #42b983;
}
</style>
```

### 三、新语法

#### 1、script setup

```vue
<script setup>
const msg = "This is Vue3 Demo";
</script>
<template>
  <h1>{{ msg }}</h1>
</template>
<style scoped>
</style>
```

#### 2、defineProps

类似于**Vue2**中的`props`，用于定义从父组件中传入的`props`参数。

```App.vue
<script setup>
import HelloWorld from "./components/HelloWorld.vue"
</script>
<template>
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>
<style>
</style>
```

```HelloWorld.vue
<script setup>
  defineProps({
    msg: String
  })
</script>
<template>
  <h1>{{ msg }}</h1>
</template>
```

#### 3、defineEmits

类似于**Vue2**中的`$emit`，用于定义子组件向父组件通信时需要触发的事件。

```App.vue
<script setup>
import HelloWorld from './components/HelloWorld.vue'

function appHandleCount() {
  console.log("app count is change");
}
</script>
<template>
  <HelloWorld @handleCount="appHandleCount" />
</template>
```

```HelloWorld.vue
<script setup>
  const emits = defineEmits(["handleCount"]);

  function addCount() {
    emits("handleCount")
  }
</script>
<template>
  <button type="button" @click="addCount">add count</button>
</template>
```

#### 4、其它对照

|      | **Vue2**     | **Vue3**         |
| ------------- |-------------|------------- |
| **computed** | 直接使用 | import { computed } from 'vue'|
| **watch** | 直接使用 | import { watch } from 'vue'|
| **nextTick** | this.$nextTick | import { nextTick } from 'vue '|
| **prpos** | 直接使用 | defineProps |
| **emit** | this.$emit | defineEmits |
