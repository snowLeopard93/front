
### 一、Vue

#### 1、值更新之后，视图未实时更新

**原因：**

该值是新增的属性，Vue无法探测新增的属性，导致视图没有实时更新；


### 二、配置

#### 1、代码打包之后，无法修改接口地址

##### 方法一：

**（1）在public文件夹下新建一个`config.js`**

```config.js
window.baseUrl = "http://localhost:8080/vue-web";
```

**（2）在`index.html`中引入该js**

```index.html
<script src="<%= BASE_URL %>config.js" charset="utf-8"></script>
```

**（3）打包之后`config.js`会单独放在`dist`目录下**