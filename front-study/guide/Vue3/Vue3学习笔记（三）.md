### 一、新语法

#### 1、setup函数

在`beforeCreate`钩子之前调用。

```vue
<template>
  <div>{{ content }}</div>
</template>
<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs } from 'vue'
import axios from 'axios'

export default defineComponent({
  name: 'Layout',
  setup() {
    const state = reactive({
      content: ''
    })
    const stateAsRefs = toRefs(state)
    const getData = () => {
      axios.get('getMockData').then(response => {
        if (response) {
          state.content = response.data;
        }
      })
    }

    onMounted(() => {
      getData();
    })

    return {
      ...state,
      ...stateAsRefs
    }
  }
})
</script>
```

```vue
<template>
  <div>{{ content }}</div>
</template>
<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs } from 'vue'
import axios from 'axios'

export default defineComponent({
  name: 'Layout',
  setup() {
    console.log("This is setup")
    const state = reactive({
      content: 'aaa'
    })
    const stateAsRefs = toRefs(state)

    onMounted(() => {
      console.log("This is onMounted")
    })
    return {
      ...state,
      ...stateAsRefs
    }
  },
  beforeCreate() {
    console.log("This is beforeCreate")
  },
  created() {
    console.log("This is created")
  }
})
</script>

// 输出结果为
// This is setup
// This is beforeCreate
// This is created
// This is beforeMount
// This is onMounted
// This is mounted
```

**参考：**

[Vue3 setup](https://v3.cn.vuejs.org/api/options-composition.html#setup)

[Vue3 组合式API setup](https://v3.cn.vuejs.org/api/composition-api.html#setup)
