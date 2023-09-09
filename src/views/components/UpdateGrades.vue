<template>
    <h3>成绩管理</h3>
    <el-table :data="filterTableData" stripe style="width: 100%;" :default-sort="{ prop: 'grade', order: 'descending' }"
        max-height="500">
        <el-table-column label="项目" prop="item" :filters="itemFilter" :filter-method="itemFilterHandler" />
        <el-table-column label="国家" prop="country" :filters="countryFilter" :filter-method="countryFilterHandler" />
        <el-table-column label="性别" prop="sex" :filters="sexFilter" :filter-method="sexFilterHandler" />
        <el-table-column label="姓名" prop="name" />
        <el-table-column label="成绩" prop="grade">
            <template #default="scope">
                <el-input v-model="scope.row.grade" v-if="scope.row.edit" placeholder="请输入成绩"></el-input>
                <div v-else>{{ scope.row.grade }}</div>
            </template>
        </el-table-column>
        <el-table-column>
            <template #header>
                <el-input v-model="search" size="small" placeholder="Type to search" />
            </template>
            <template #default="scope">
                <el-button @click="handleEdit(scope.$index, scope.row)" type="primary">编辑</el-button>
                <el-popconfirm confirm-button-text="确认" cancel-button-text="取消" title="确认修改成绩吗?"
                    @confirm="handleConfirm(scope.$index, scope.row)" @cancel="scope.row.grade = beforeGrade">
                    <template #reference>
                        <el-button type="success" @click="scope.row.edit = false">保存</el-button>
                    </template>
                </el-popconfirm>
            </template>
        </el-table-column>
    </el-table>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const props = defineProps({
    tableData: {
        type: Object,
        default: [
            {
                item: '乒乓球',
                country: '中国',
                sex: '男',
                name: '张继科',
                grade: 98
            }
        ]
    }
})

const emits = defineEmits({
    ModifyGrade: null
})

//可搜索表格
const search = ref('')

const filterTableData = computed(() =>
    props.tableData.filter(
        (data) => !search.value ||
            data.name.includes(search.value) ||
            data.country.includes(search.value) ||
            data.item.includes(search.value) ||
            data.sex.includes(search.value)
    )
)

//项目筛选
const itemFilter = computed(() => {
    const tmp = reactive([])
    for (let data of props.tableData) {
        tmp.push({ text: data.item, value: data.item })
    }
    //使用Map+filter去重，若Map中存在value则筛出，否则加上键值对{value:1}
    const res = new Map();
    return tmp.filter((data) =>
        !res.has(data.value) && res.set(data.value, 1)
    )
})

const itemFilterHandler = (value, row, col) => {
    return row.item == value
}

//国家筛选
const countryFilter = computed(() => {
    const tmp = reactive([])
    for (let data of props.tableData) {
        tmp.push({ text: data.country, value: data.country })
    }
    //使用Map+filter去重，若Map中存在value则筛出，否则加上键值对{value:1}
    const res = new Map();
    return tmp.filter((data) =>
        !res.has(data.value) && res.set(data.value, 1)
    )
})

const countryFilterHandler = (value, row, col) => {
    return row.country == value
}

//性别筛选
const sexFilter = reactive([{ text: '男', value: '男' }, { text: '女', value: '女' }])

const sexFilterHandler = (value, row, col) => {
    return row.sex == value
}

//编辑删除
let beforeGrade
const handleEdit = (index, row) => {
    row.edit = true
    beforeGrade = row.grade
}

const handleConfirm = (index, row) => {
    //console.log(index, row)
    console.log(props.tableData);
    emits('ModifyGrade', row)
}

</script>

<style scope>
.el-table,
.el-table tr,
.el-table td,
.el-table th {
    background-color: transparent !important;
}
</style>