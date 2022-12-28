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

## 进行页面填充模块

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

效果：

![image-20221227110122274](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271101898.png)

## 页面优化

第一步：标题居中

```css
.title {
    text-align: center;
}
```

第二步：搜索框和增加按钮在同一列中

```css
.query-box {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.el-input {
    width: 200px;
}
```

第三步：增加一个多选框

```vue
<el-table border ref="multipleTableRef" :data="tableData" style="width: 100%"
            @selection-change="handleSelectionChange">
<el-table-column type="selection" width="55" />
```

![image-20221227111419673](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271114241.png)

第四步：在script里面添加方法

```vue
<script setup>
import { ref } from "vue";
let multipleSelection = ref([])
const handleSelectionChange = (val) => {
    multipleSelection.value = val
    console.log(val)
}
</script>
```

效果：

![image-20221227111615040](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271116437.png)

## 新增弹窗

第一步：引入弹窗组件

```vue
<el-dialog v-model="dialogFormVisible" title="Shipping address">
            <el-form :model="tableForm">
                <el-form-item label="姓名" :label-width="80">
                    <el-input v-model="tableForm.name" autocomplete="off" />
                </el-form-item>
            </el-form>
            <el-form :model="tableForm">
                <el-form-item label="邮箱" :label-width="80">
                    <el-input v-model="tableForm.email" autocomplete="off" />
                </el-form-item>
            </el-form>
            <el-form :model="tableForm">
                <el-form-item label="电话" :label-width="80">
                    <el-input v-model="tableForm.phone" autocomplete="off" />
                </el-form-item>
            </el-form>
            <el-form :model="tableForm">
                <el-form-item label="地址" :label-width="80">
                    <el-input v-model="tableForm.address" autocomplete="off" />
                </el-form-item>
            </el-form>
            <el-form :model="tableForm">
                <el-form-item label="状态" :label-width="80">
                    <el-input v-model="tableForm.state" autocomplete="off" />
                </el-form-item>
            </el-form>
            <template #footer>
                <span class="dialog-footer">
                    <el-button type="primary" @click="dialogFormVisible = false">
                        确认
                    </el-button>
                </span>
            </template>
</el-dialog>
```

第二步：在增加按钮添加个事件

```vue
<el-button type="primary" @click="handleAdd">增加</el-button>
```

第三步：在script中添加方法

```vue
let dialogFormVisible = ref(false)
let tableForm = ref({
    name: '张三',
    email: '123@gmail.com',
    phone: '13888888888',
    state: '在职',
    address: '广东省'
})

const handleAdd = () => {
    dialogFormVisible.value = true
}
```

效果：

![image-20221227114128813](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271141342.png)

## 新增数据

第一步：

```javascript
const dialogConfirm = () => {
    dialogFormVisible.value = false
    // 1.拿到数据
    // 2.添加到table
    tableData.value.push({
        id: (tableData.value.length + 1).toString(),
        ...tableForm.value
    })
}
```

效果：

![image-20221227153344052](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271533214.png)

## 删除数据

第一步：找到删除按钮对应的事件

![image-20221227160541778](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271605385.png)

第二步：传参数

```javascript
const handleRowDel = ({ id }) => {  // 删除对应事件
    console.log(id)
    // 1.通过id获取到条目对应的索引值
    let index = tableData.value.findIndex(item => item.id === id)
    // 2.通过索引值进行删除对应条目
    tableData.value.splice(index, 1)
}
```

效果：

![image-20221227162702038](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271627184.png)

## 多选删除

第一步：增加个多选删除的按钮

![image-20221227175301572](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271753538.png)

第二步：选中获取单个id

```javascript
const handleSelectionChange = (val) => {
    multipleSelection.value = []    // 进行清空
    val.forEach(item => {   // 遍历获取每个id
        multipleSelection.value.push(item.id)
    })
    console.log(multipleSelection);
}
```

第三步：删除单条

```javascript
// 删除一条
const handleRowDel = ({ id }) => {  // 删除对应事件
    console.log(id)
    // 1.通过id获取到条目对应的索引值
    let index = tableData.value.findIndex(item => item.id === id)
    // 2.通过索引值进行删除对应条目
    tableData.value.splice(index, 1)
}
```

第四步：遍历删除多条

```javascript
const handleDelList = () => {
    multipleSelection.value.forEach(id => {
        handleRowDel({ id })
    })
    multipleSelection.value = []
}
```

效果：

![image-20221227175659390](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212271757435.png)

## 编辑数据

第一步：在编辑按钮增加click事件

```vue
<el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
```

![image-20221228094707533](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212280947871.png)

第二步：编写编辑方法

```vue
const handleEdit = (row) => {
    dialogFormVisible.value = true  // 显示对话框
    dialoyType.value = 'edit'   // 定义为编辑
    console.log(row);
    tableForm.value = { ...row }
}
```

第三步：进行确定事件判断

```vue
const dialogConfirm = () => {
    dialogFormVisible.value = false // 关闭对话框
    // 1、判断是新增还是编辑
    if (dialoyType.value === 'add') {
        // 1.拿到数据
        // 2.添加到table
        tableData.value.push({  // 将对话框数据push到表格中
            id: (tableData.value.length + 1).toString(),    // 模拟id数据，进行获取table表格行数+1的到最后的id，并转为字符串
            ...tableForm.value
        })
        console.log(tableData);
    } else {
        // 1、获取到当前的这条的索引
        let index = tableData.value.findIndex(item => item.id === tableForm.value.id)
        console.log(index)
        // 2、替换当前索引值对应的数据
        tableData.value[index] = tableForm.value
    }
}
```

效果：

![image-20221228094939930](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212280949907.png)

![image-20221228094955617](https://cdn.jsdelivr.net/gh/ranyong1997/image_collect@main/img/202212280949716.png)

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