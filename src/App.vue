<!--
 * @Descripttion: 
 * @version: 
 * @Author: 冉勇
 * @Date: 2022-12-27 09:50:33
 * @LastEditTime: 2022-12-28 10:29:30
-->
<script setup>
import { ref } from "vue";
// 数据
let queryInput = ref("")
let tableData = ref([
    {
        id: '1',
        name: 'Tom1',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '2',
        name: 'Tom2',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '3',
        name: 'Tom3',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        id: '4',
        name: 'Tom4',
        email: '123qq.com',
        phone: '13888888888',
        state: 'California',
        address: 'No. 189, Grove St, Los Angeles',
    }])
let tableDataCopy = Object.assign(tableData)    // 浅拷贝
let multipleSelection = ref([]) // 多次选择
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
// 搜索
const handleQueryName = (val) => {
    // console.log(queryInput.value)    // 监听用户输入数据
    // console.log(val) // 监听用户输入数据
    // todo: bug，如果搜索一次可成功，多次搜索就没有数据展示
    if (val.length > 0) {
        tableData = tableData.value.filter(item => (item.name).toLowerCase().match(val.toLowerCase()))
        console.log(tableData)
    } else {
        tableData = tableDataCopy
    }
}
// 编辑
const handleEdit = (row) => {
    dialogFormVisible.value = true  // 显示对话框
    dialoyType.value = 'edit'   // 定义为编辑
    // console.log(row);
    tableForm.value = { ...row }
}
// 删除一条
const handleRowDel = ({ id }) => {  // 删除对应事件
    // console.log(id)
    // 1.通过id获取到条目对应的索引值
    let index = tableData.value.findIndex(item => item.id === id)
    // 2.通过索引值进行删除对应条目
    tableData.value.splice(index, 1)
}
// 删除多条
const handleDelList = () => {
    multipleSelection.value.forEach(id => {
        handleRowDel({ id })
    })
    multipleSelection.value = []
}
// 选中
const handleSelectionChange = (val) => {
    multipleSelection.value = []    // 进行清空
    val.forEach(item => {   // 遍历获取每个id
        multipleSelection.value.push(item.id)
    })
    // console.log(multipleSelection);
}
// 点击增加
const handleAdd = () => {
    dialogFormVisible.value = true  // 显示对话框
    tableForm.value = {}    // 数据清空
    dialoyType.value = 'add'    // 定义为新增
}
// 点击确定
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
        // console.log(tableData);
    } else {
        // 1、获取到当前的这条的索引
        let index = tableData.value.findIndex(item => item.id === tableForm.value.id)
        // console.log(index)
        // 2、替换当前索引值对应的数据
        tableData.value[index] = tableForm.value
    }
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
            <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" @input="handleQueryName" />
            <div class="btn-list">
                <el-button type="primary" @click="handleAdd">增加</el-button>
                <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length > 0">删除多选</el-button>
            </div>
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
                    <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
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
