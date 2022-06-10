### 一、表单组件

#### 1、单选按钮

**（1）点击切换选中时有触发change事件，但未更新选中状态**

```
/* v-model 不能写成:value */
<a-radio-group v-model="value" @change="onChange">
    <a-radio :value="1">
    A
    </a-radio>
    <a-radio :value="2">
    B
    </a-radio>
    <a-radio :value="3">
    C
    </a-radio>
    <a-radio :value="4">
    D
    </a-radio>
</a-radio-group>
```

#### 2、日期组件

**（1）日期组件无年份选择器**

**解决思路：** 借助`FormModel`组件实现赋值

```vue
<template>
    <div>
        <a-form-model ref="searchForm" :model="searchForm">
            <a-date-picker
                placeholder="请选择年份"
                mode="year"
                format="YYYY"
                v-model="searchForm.searchYear"
                :inputReadOnly="true"
                @panelChange="handleSearchYearChange"
                style="width: 100%"
            />
        </a-form-modal>
    </div>
</template>
<script>
    export default {
        name: "Report",
        data() {
            return {
                searchForm: {
                    searchYear: null,
                }
            }
        },
        methods: {
            handleSearchYearChange() {
                this.searchForm.searchYear = value;
            }
        }
      },
</script>
```

### 二、表格组件

#### 1、columns

**（1）设置`ellipsis`为true之后，鼠标悬浮无提示信息**

需要确保显示的值是字符串。

```vue
<template>
    <div>
        <a-table
        :columns="columns"
        :data-source="dataSource"
        :pagination="false"
      />
    </div>
</template>
<script>
export default {
    name: "MyTable",
    data() {
        return {
            columns: [
                {
                title: "名称",
                width: "20%",
                align: "center",
                dataIndex: "appName",
                ellipsis: true,
              },
              {
                title: "平均处理时长(单位:ms)",
                width: "20%",
                align: "center",
                dataIndex: "resTime",
                ellipsis: true,
                customRender: function (text, record) {
                  return record.resTime.toString();
                },
              },
            ],
            dataSource: [{
                name: "系统1",
                resTime: 10
            },{
                name: "系统2",
                resTime: 20
            }]
        }
    },
    methods: {

    }
}
</script>
```