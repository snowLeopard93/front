
### 一、Form表单 前置

#### 1、引入Form

```main.js
import {
  Form,
  Input
} from "ant-design-vue";

Vue.use(Form);
Vue.use(Input);
```

#### 2、基本布局

```BasicForm.vue
<template>
  <div>
    <a-form :layout="formLayout">
      <a-form-item
        label="Field A"
        :label-col="formItemLayout.labelCol"
        :wrapper-col="formItemLayout.wrapperCol"
      >
        <a-input placeholder="input placeholder" />
      </a-form-item>
      <a-form-item
        label="Field B"
        :label-col="formItemLayout.labelCol"
        :wrapper-col="formItemLayout.wrapperCol"
      >
        <a-input placeholder="input placeholder" />
      </a-form-item>
      <a-form-item :wrapper-col="buttonItemLayout.wrapperCol">
        <a-button type="primary">
          Submit
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
export default {
  name: "BasicForm",
  data() {
    return {
      formLayout: "horizontal"
    };
  },
  computed: {
    formItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            labelCol: { span: 4 },
            wrapperCol: { span: 14 }
          }
        : {};
    },
    buttonItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            wrapperCol: { span: 14, offset: 4 }
          }
        : {};
    }
  },
  methods: {
    handleFormLayoutChange(e) {
      this.formLayout = e.target.value;
    }
};
</script>
```

### 二、文本框

#### 1、校验

**（1）使用`validateStatus`**

```BasicForm.vue
<template>
  <div>
    <a-form :layout="formLayout">
      <a-form-item
        label="Field A"
        :label-col="formItemLayout.labelCol"
        :wrapper-col="formItemLayout.wrapperCol"
        :validateStatus="fieldAStatus"
        :help="fieldAHelp"
      >
        <a-input placeholder="input placeholder" @change="handleFieldAChange" />
      </a-form-item>
      <a-form-item
        label="Field B"
        :label-col="formItemLayout.labelCol"
        :wrapper-col="formItemLayout.wrapperCol"
        :validateStatus="fieldBStatus"
        :help="fieldBHelp"
      >
        <a-input placeholder="input placeholder" @change="handleFieldBChange" />
      </a-form-item>
      <a-form-item :wrapper-col="buttonItemLayout.wrapperCol">
        <a-button type="primary">
          Submit
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
export default {
  name: "BasicForm",
  data() {
    return {
      formLayout: "horizontal",
      fieldAStatus: "",
      fieldAHelp: "",
      fieldBStatus: "",
      fieldBHelp: ""
    };
  },
  computed: {
    formItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            labelCol: { span: 4 },
            wrapperCol: { span: 14 }
          }
        : {};
    },
    buttonItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            wrapperCol: { span: 14, offset: 4 }
          }
        : {};
    }
  },
  methods: {
    handleFormLayoutChange(e) {
      this.formLayout = e.target.value;
    },
    handleFieldAChange(e) {
      if (e.target.value.length <= 5) {
        this.fieldAStatus = "error";
        this.fieldAHelp = "必须大于5个字符";
      } else {
        this.fieldAStatus = "";
        this.fieldAHelp = "";
      }
    },
    handleFieldBChange(e) {
      if (e.target.value.length <= 5) {
        this.fieldBStatus = "error";
        this.fieldBHelp = "必须大于5个字符";
      } else {
        this.fieldBStatus = "";
        this.fieldBHelp = "";
      }
    }
  }
};
</script>
```

**（2）使用`v-decorator`**

```BasicForm.vue
<template>
  <div>
    <a-form :layout="formLayout" :form="form" @submit="handleSubmit">
      <a-form-item
        label="Field A"
        :label-col="formItemLayout.labelCol"
        :wrapper-col="formItemLayout.wrapperCol"
        has-feedback
      >
        <a-input
          placeholder="input placeholder"
          v-decorator="[
            'fieldA',
            {
              rules: [
                {
                  required: true,
                  message: '不能为空！'
                },
                {
                  validator: validateFieldA
                }
              ]
            }
          ]"
        />
      </a-form-item>
      <a-form-item :wrapper-col="buttonItemLayout.wrapperCol">
        <a-button type="primary" html-type="submit">
          Submit
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
export default {
  name: "BasicForm",
  data() {
    return {
      formLayout: "horizontal"
    };
  },
  beforeCreate() {
    this.form = this.$form.createForm(this, { name: "basicForm" });
  },
  computed: {
    formItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            labelCol: { span: 4 },
            wrapperCol: { span: 14 }
          }
        : {};
    },
    buttonItemLayout() {
      const { formLayout } = this;
      return formLayout === "horizontal"
        ? {
            wrapperCol: { span: 14, offset: 4 }
          }
        : {};
    }
  },
  methods: {
    validateFieldA(rule, value, callback) {
      if (value && value.length <= 5) {
        callback("长度不能小于5");
      }
      callback();
    },
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFieldsAndScroll((err, values) => {
        if (!err) {
          console.log("Received values of form: ", values);
        }
      });
    }
  }
};
</script>
```

### 三、FormModel表单 前置

**参考：**

[ant-design-vue Form表单](https://www.antdv.com/components/form-cn)