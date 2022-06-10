### 一、新特性

#### 1、组合式 API

#### 2、Teleport

#### 3、片段

#### 4、触发组件选项

#### 5、来自 @vue/runtime-core 的 createRenderer API，用于创建自定义渲染器

#### 6、单文件组件组合式 API 语法糖 (script setup)

#### 7、单文件组件状态驱动的 CSS 变量 (style中的 v-bind)

#### 8、SFC style scoped 现在可以包含全局规则或只针对插槽内容的规则

### 二、变更

#### 1、全局API

**（1）`createApp`**

```
import { createApp } from 'vue'

const app = createApp({})
```

**（2）`nextTick`**
```
import { nextTick } from 'vue'

nextTick(() => {
  // 一些和DOM有关的东西
})
```

| 2.x 全局 API | 	3.x 实例 API (app) | 
| ------------- |-------------|
| Vue.config | 	app.config | 
| Vue.config.productionTip | 	removed  | 
| Vue.config.ignoredElements | 	app.config.compilerOptions.isCustomElement  | 
| Vue.component	|  app.component | 
| Vue.directive	|  app.directive | 
| Vue.mixin	|  app.mixin | 
| Vue.use	|  app.use (见下方) | 
| Vue.prototype	|  app.config.globalProperties  | 
| Vue.extend	|  removed | 

#### 2、模板指令

**（1）`v-model`**

**绑定多个`v-model`**

```
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />

<!-- 是以下的简写： -->

<ChildComponent
  :title="pageTitle"
  @update:title="pageTitle = $event"
  :content="pageContent"
  @update:content="pageContent = $event"
/>
```

**（2）`key`**

```
<!-- Vue 2.x -->
<template v-for="item in list">
  <div :key="'heading-' + item.id">...</div>
  <span :key="'content-' + item.id">...</span>
</template>
```

```
<!-- Vue 3.x -->
<template v-for="item in list" :key="item.id">
  <div>...</div>
  <span>...</span>
</template>
```

**（3）`v-if`和`v-for`的优先级**

2.x 版本中在一个元素上同时使用 `v-if` 和 `v-for` 时，`v-for` 会优先作用。

3.x 版本中 `v-if` 总是优先于 `v-for` 生效。

**（4）`v-bind`合并行为**

```
<!-- Vue 2.x 覆盖 -->
<!-- template -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- result -->
<div id="red"></div>
```

```
<!-- Vue 2.x 合并到object -->
<!-- template -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- result -->
<div id="blue"></div>

<!-- template -->
<div v-bind="{ id: 'blue' }" id="red"></div>
<!-- result -->
<div id="red"></div>
```

**（5）移除`v-on.native`修饰符**

**（6）`v-for` 中的 Ref 数组**

```
<div v-for="item in list" :ref="setItemRef"></div>

export default {
  data() {
    return {
      itemRefs: []
    }
  },
  methods: {
    setItemRef(el) {
      if (el) {
        this.itemRefs.push(el)
      }
    }
  },
  beforeUpdate() {
    this.itemRefs = []
  },
  updated() {
    console.log(this.itemRefs)
  }
}
```

#### 3、组件

#### 4、渲染函数

#### 5、自定义元素

#### 6、小改变

**（1）** destroyed 生命周期选项被重命名为 unmounted

**（2）** beforeDestroy 生命周期选项被重命名为 beforeUnmount

**（3）** default prop 工厂函数不再可以访问 this 上下文

**（4）** 自定义指令的 API 已更改为与组件生命周期一致，且 binding.expression 已移除

**（5）** data 选项应始终被声明为一个函数

**（6）** 来自 mixin 的 data 选项现在为浅合并

**（7）** Attribute 强制策略已更改

**（8）** 一些过渡的 class 被重命名

**（9）** <TransitionGroup> 不再默认渲染包裹元素

**（10）** 当侦听一个数组时，只有当数组被替换时，回调才会触发，如果需要在变更时触发，则必须指定 deep 选项

**（11）** 没有特殊指令的标记 (v-if/else-if/else、v-for 或 v-slot) 的 <template> 现在被视为普通元素，并将渲染为原生的 <template> 元素，而不是渲染其内部内容。

**（12）** 已挂载的应用不会取代它所挂载的元素

**（13）** 生命周期的 hook: 事件前缀改为 vnode-

#### 7、移除API


**推荐：**

[Vue3](https://v3.cn.vuejs.org/guide/migration/introduction.html)