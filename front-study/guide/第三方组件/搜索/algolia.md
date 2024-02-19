## 一、准备

### 1、创建账号

可以使用github账号[登录](https://dashboard.algolia.com/users/sign_in)，也可以使用其他账号

![algolia login](../../../images/algolia/algolia-login.png)

### 2、创建应用

创建应用后会得到一个应用id，在后续的操作中，需要用到该应用id，定义为`VUE_APP_ALGOLIA_APPLICATION_ID`。

![algolia create application](../../../images/algolia/algolia-create-application.png)

### 3、创建索引

![algolia create index](../../../images/algolia/algolia_create_index.png)

创建索引后会得到一个索引名，在后续的操作中，需要用到该索引名，定义为`VUE_APP_ALGOLIA_INDEX_NAME`。

![algolia index](../../../images/algolia/algolia_index.png)

### 4、查看api-key

（1）点击`Overview`菜单

（2）点击`API Keys`菜单

![algolia API keys](../../../images/algolia/algolia_API_keys.png)

### 5、上传数据

## 二、安装依赖

### 1、安装`algoliasearch`

```bash
npm install algoliasearch
```

### 2、安装`vue-instantsearch`
```bash
npm install vue-instantsearch
```

## 三、使用（以vue3为例）

### 1、通过组件调用

#### 1.1 在`main.js`中引入

```javascript
import InstantSearch from 'vue-instantsearch/vue3/es';

const app = createApp(App);
app.use(InstantSearch);
app.mount('#app');
```

#### 1.2 变量配置

将需要用到的变量定义在.env配置文件中：

应用id：`VUE_APP_ALGOLIA_APPLICATION_ID`

索引名：`VUE_APP_ALGOLIA_INDEX_NAME`

apiKey：`VUE_APP_ALGOLIA_API_KEY`（此处使用的是查询key）

查询服务ip：`VUE_APP_ALGOLIA_SEARCH_HOST`，值为：`https://VUE_APP_ALGOLIA_APPLICATION_ID-dsn.algolia.net`，`VUE_APP_ALGOLIA_APPLICATION_ID`需要替换为具体值。

#### 1.3 使用`ais-instant-search`组件

```vue
<template>
  <ais-instant-search
    :search-client="searchClient"
    :index-name="indexName">
    <ais-configure :hits-per-page.camel="5" />
    <ais-search-box placeholder="请输入搜索关键词" />
    <div style="margin-top: 10px;">
      <ais-stats>
        <template #default="{ nbHits, processingTimeMS }">
          共有 {{ nbHits }} 条记录，耗时 {{ processingTimeMS }}ms
        </template>
      </ais-stats>
    </div>
    <ais-hits>
      <template #item="{ item }">
        <div>
          <ais-highlight :hit="item" attribute="name" />
        </div>
      </template>
    </ais-hits>

    <div style="display: flex;justify-content: center;margin-top: 10px;">
      <ais-pagination />
    </div>
  </ais-instant-search>
</template>
<script setup>
import algoliasearch from 'algoliasearch/lite';
import 'instantsearch.css/themes/satellite-min.css';

const indexName = ref(process.env.VUE_APP_ALGOLIA_INDEX_NAME);
const appId = ref(process.env.VUE_APP_ALGOLIA_APPLICATION_ID);
const apiKey = ref(process.env.VUE_APP_ALGOLIA_API_KEY);

const searchClient = ref(undefined);
searchClient.value = algoliasearch(
  appId.value,
  apiKey.value,
);
</script>
```

### 2、通过接口调用

#### 2.1 查询数据接口

官方文档在此：[algolia search api](https://www.algolia.com/doc/rest-api/search/#search-index-post)

```javascript
export function searchData(params) {
  return request.POST(`${process.env.VUE_APP_ALGOLIA_SEARCH_HOST}/1/indexes/${process.env.VUE_APP_ALGOLIA_INDEX_NAME}/query`, params);
}
```

其中：

（1）`request`为通过`axios`封装的请求对象。

（2）接口请求参数格式如下：

```json
{
  "query": "nginx", // 查询关键字
  "hitsPerPage": 10, // 每页返回的数据
  "page": 0 // 第几页，默认为0，表示第一页
}
```

更多接口请求参数请点击查看[algolia search params](https://www.algolia.com/doc/api-reference/search-api-parameters/)

（3）请求头配置

```json
{
  "X-Algolia-Api-Key": '',
  "X-Algolia-Application-Id": ''
}
```

（4）更多接口请点击查看[algolia search api](https://www.algolia.com/doc/rest-api/search/)。

#### 2.2 页面使用

```vue
<template>
  <div class="basic-search">
    <div style="height: 420px;overflow-y: auto;">
       <div class="hit-list">
          <div v-for="item in hits" :key="item.key" class="hit-item">
            <div v-if="item._highlightResult" v-html="item._highlightResult.name.value" />
            <div v-else>{{ item.name }}</div>
          </div>
        </div>
      </div>
      <div style="height: 50px;display: flex;justify-content: center;">
        <el-pagination
          v-model:current-page="currentPage"
          background
          layout="prev, pager, next"
          :total="total"
          :page-size="pageSize"
          :hide-on-single-page="true"
          @current-change="changeCurrent"
        />
      </div>
      <div style="float: right">
        <ais-powered-by />
      </div>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import { searchData } from '@/api/index';

const currentPage = ref(1);
const total = ref(0);
const pageSize = ref(10);
const hits = ref([]);

const getData = () => {
  const perPage = pageSize.value;
  const queryPage = currentPage.value - 1;
  const params = { query: 'test', hitsPerPage: perPage, page: queryPage };
  searchData(params).then((res) => {
    if (res.hits) {
      hits.value = res.hits;
    }
    total.value = res.nbHits;
  });
};

const changeCurrent = () => {
  getData();
};
</script>
<style lang="scss" scoped>
.basic-search {
  height: 500px;
}
.search-container {
  height: calc(100% - 46px);
  width: 100%;
  overflow-y: hidden;
  position: absolute;
  top: 46px;
}

:deep(.ais-Highlight-highlighted, .ais-Snippet-highlighted) {
  color: #1989fa;
  background: #FFFFFF;
}

:deep(em) {
  color: #1989fa;
  background-color: #FFFFFF;
  font-style: normal;
}

.hit-list {
  padding: 20px;
  .hit-item {
    margin-bottom: 20px;
    cursor: pointer;
  }
}

</style>
```