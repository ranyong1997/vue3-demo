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