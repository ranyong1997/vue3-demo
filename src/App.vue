<!--
 * @Descripttion: 
 * @version: 
 * @Author: 冉勇
 * @Date: 2022-12-27 09:50:33
 * @LastEditTime: 2022-12-27 16:09:39
-->
<script setup>
import { ref } from "vue";
// 数据
let queryInput = ref("")
let tableData = ref([
    {
        id: '1',
        name: 'Tom',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '2',
        name: 'Tom',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '3',
        name: 'Tom',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '4',
        name: 'Tom',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    }])
let multipleSelection = ref([])
let dialogFormVisible = ref(false)
let tableForm = ref({
    name: '张三',
    email: '123@gmail.com',
    phone: '13888888888',
    state: '在职',
    address: '广东省'
})
let dialoyType = ref('add')
// 方法
const handleRowDel = ({ id }) => {  // 删除对应事件
    console.log(id)
    // 1.通过id获取到条目对应的索引值
    let index = tableData.value.findIndex(item => item.id === id)
    // 2.通过索引值进行删除对应条目
    tableData.value.splice(index, 1)
}
const handleSelectionChange = (val) => {
    multipleSelection.value = val
    console.log(val)
}
// 点击增加
const handleAdd = () => {
    dialogFormVisible.value = true  // 显示对话框
    tableForm.value = {}    // 数据清空
}
// 点击确定
const dialogConfirm = () => {
    dialogFormVisible.value = false // 关闭对话框
    // 1.拿到数据
    // 2.添加到table
    tableData.value.push({  // 将对话框数据push到表格中
        id: (tableData.value.length + 1).toString(),    // 模拟id数据，进行获取table表格行数+1的到最后的id，并转为字符串
        ...tableForm.value
    })
    console.log(tableData);
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
            <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" />
            <el-button type="primary" @click="handleAdd">增加</el-button>
        </div>
        <!-- 表格 -->
        <el-table border ref="multipleTableRef" :data="tableData" style="width: 100%"
            @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55" />
            <el-table-column prop="name" label="姓名" width="120" />
            <el-table-column prop="email" label="邮箱" width="120" />
            <el-table-column prop="phone" label="电话" width="120" />
            <el-table-column prop="state" label="状态" width="120" />
            <el-table-column prop="address" label="地址" width="300" />
            <el-table-column fixed="right" label="操作" width="120">
                <template #default="scope">
                    <el-button link type="primary" size="small" @click="handleRowDel(scope.row)"
                        style="color:#F56C6C">删除</el-button>
                    <el-button link type="primary" size="small">编辑</el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- 弹窗 -->
        <el-dialog v-model="dialogFormVisible" :title="dialoyType === 'add' ? '新增' : '编辑'">
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
                    <el-button type="primary" @click="dialogConfirm">
                        确认
                    </el-button>
                </span>
            </template>
        </el-dialog>
    </div>
</template>

<style scoped>
.table-box {
    width: 800px;
    margin: 200px auto;

}

.title {
    text-align: center;
}

.query-box {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.query-input {
    width: 200px;
}
</style>
