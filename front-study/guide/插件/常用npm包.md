### 一、特效

#### 1、perfect-scrollbar

##### 1.1 引入

```sh
npm i perfect-scrollbar
```

##### 1.2 基本用法

```javascript
import "@/assets/styles/perfect-scrollbar.css";
```

```vue
<template>
    <div>
        <div ref="popupBox">
            <div v-for="item in list" :key="item.id">
                {{ item.name }}
            </div>
        </div>
    </div>
</template>
<script>
import PerfectScrollbar from "perfect-scrollbar";
export default {
    name: "BasicScrollBar"
    data() {
        return {
            perfectScrollBar: null,
        }
    },
    watch: {
        list() {
            this.$nextTick(() => {
                if (!this.perfectScrollBar) {
                    this.perfectScrollBar = new PerfectScrollbar(this.$refs.popupBox);
                } else {
                    this.perfectScrollBar.update();
                }
            })
        }
    },
    mounted() {
        this.init();
    },
    beforeDestroy() {
        if(!this.perfectScrollBar) {
            this.perfectScrollBar.destroy();
            this.perfectScrollBar = null;
        }
    },
    methods: {
        init() {
            this.list = [{id: 1, name: "a"}, {id: 2, name: "b"}, {id: 3, name: "c"}];
        }
    }
}
</script>
```



