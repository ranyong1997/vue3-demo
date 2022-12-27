<!--
 * @Descripttion: 
 * @version: 
 * @Author: 冉勇
 * @Date: 2022-12-27 09:50:33
 * @LastEditTime: 2022-12-27 10:08:31
-->
<h1>vue3-Demo 一个CRUD练手项目</h1>
## 安装Element-Plus

官网：https://element-plus.gitee.io/zh-CN

第一步：安装：

```shell
pnpm install element-plus
```

第二步：在main.js中引入

```javascript
# main.js

import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```

![image-20221227102100591](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271021468.png)

第三步：在项目中测试使用

```vue
# App.vue

<template>
    <el-row class="mb-4">
        <el-button>Default</el-button>
        <el-button type="primary">Primary</el-button>
        <el-button type="success">Success</el-button>
        <el-button type="info">Info</el-button>
        <el-button type="warning">Warning</el-button>
        <el-button type="danger">Danger</el-button>
    </el-row>
</template>
```

![image-20221227102133053](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271021375.png)

效果：

![image-20221227102147659](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271021573.png)

### 进行页面填充模块

![简易版CRUD图](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271058838.jpg)

完整代码：

```vue
# App.vue

<script setup>
import { ref } from "vue";
// 数据
let queryInput = ref("")
let tableData = ref([{
    date: '2016-05-03',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
},
{
    date: '2016-05-02',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
},
{
    date: '2016-05-04',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
},
{
    date: '2016-05-01',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
}])
// 方法
const handleRowClick = () => {
    console.log('click')
}
</script>

<template>
    <div class="table-box">
        <!-- 标题 -->
        <div class="title">
            <h2>CRUD</h2>
        </div>
        <!-- 功能 -->
        <div class="query-box">
            <el-input v-model="queryInput" placeholder="请输入姓名搜索" />
            <el-button type="primary">增加</el-button>
        </div>
        <!-- 表格 -->
        <el-table :data="tableData" style="width: 100%">
            <el-table-column fixed prop="date" label="Date" width="150" />
            <el-table-column prop="name" label="Name" width="120" />
            <el-table-column prop="state" label="State" width="120" />
            <el-table-column prop="city" label="City" width="120" />
            <el-table-column prop="address" label="Address" width="600" />
            <el-table-column prop="zip" label="Zip" width="120" />
            <el-table-column fixed="right" label="Operations" width="120">
                <template #default>
                    <el-button link type="primary" size="small" @click="handleRowClick">Detail</el-button>
                    <el-button link type="primary" size="small">Edit</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<style scoped>
.table-box{
    width:800px;
    position: absolute;
    top:50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
</style>
```

![image-20221227110122274](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271101898.png)





## 用法

我这采用pnpm安装管理依赖
### 安装依赖
```shell
pnpm install
```
### 安装一个包
```shell
pnpm add 包名
```
### 卸载一个包
```shell
pnpm remove 包名
```