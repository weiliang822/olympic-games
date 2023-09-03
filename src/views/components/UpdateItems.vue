<template>
    <h3>项目管理</h3>
    <el-table :data="filterTableData" stripe>
        <el-table-column label="项目" prop="name">
            <template #default="scope">
                <el-input v-model="scope.row.name" v-if="scope.row.edit && scope.row.name == ''"
                    placeholder="请输入项目名称"></el-input>
                <div v-else>{{ scope.row.name }}</div>
            </template>
        </el-table-column>
        <el-table-column label="参与国家" prop="country">
            <template #default="scope">
                <el-select v-model="addcountry" v-if="scope.row.edit" placeholder="请选择新参与国家" filterable multiple>
                    <el-option v-for="item of scope.row.nocountry" :label=item :value=item></el-option>
                </el-select>
                <div v-else>{{ scope.row.country }}</div>
            </template>
        </el-table-column>
        <el-table-column label="性别" prop="sex">
            <template #default="scope">
                <el-select v-model="addsex" v-if="scope.row.edit && scope.row.nosex.length" placeholder="请选择新参与性别"
                    filterable multiple>
                    <el-option v-for="item of scope.row.nosex" :label=item :value=item></el-option>
                </el-select>
                <div v-else>{{ scope.row.sex }}</div>
            </template>
        </el-table-column>
        <el-table-column label="项目简介" prop="introduction">
            <template #default="scope">
                <el-input v-model="scope.row.introduction" v-if="scope.row.edit" placeholder="请输入项目简介" clearable></el-input>
                <div v-else>{{ scope.row.introduction }}</div>
            </template>
        </el-table-column>
        <el-table-column>
            <template #header>
                <el-button type="primary" @click="addItem">+ 新增</el-button>
                <el-input v-model="search" size="small" placeholder="Type to search" />
            </template>
            <template #default="scope">
                <el-button @click="handleEdit(scope.$index, scope.row)" type="primary">编辑</el-button>
                <el-popconfirm confirm-button-text="确认" cancel-button-text="取消" title="确认修改吗?"
                    @confirm="handleConfirm(scope.$index, scope.row)" @cancel="handleCancel(scope.$index, scope.row)">
                    <template #reference>
                        <el-button type="success" @click="handleSave(scope.$index, scope.row)">保存</el-button>
                    </template>
                </el-popconfirm>
            </template>
        </el-table-column>
    </el-table>
</template>

<script setup>
import { ref, computed, reactive } from 'vue'
import { ElNotification } from 'element-plus'

const props = defineProps({
    countries: {
        type: Object
    },
    items: {
        type: Array
    }
})

const emits = defineEmits({
    ModifyItem: null
})

//可搜索表格
const search = ref('')

const filterTableData = computed(() =>
    props.items.filter(
        (data) => !search.value ||
            data.name.includes(search.value) ||
            data.country.includes(search.value) ||
            data.sex.includes(search.value) ||
            data.introduction.includes(search.value)
    )
)

const addItem = () => {
    props.items.push({
        name: '',
        country: [],
        nocountry: ['中国', '美国', '法国', '英国'],
        sex: [],
        nosex: ['男', '女'],
        introduction: ''
    })
}

//编辑删除
const isedit = ref(false) //是否正在编辑
const addcountry = ref([]) //绑定v-model为什么一定要ref
const addsex = ref([])

let beforeIntroduction
const handleEdit = (index, row) => {
    //console.log(index, row)
    console.log(addcountry)
    if (!isedit.value) {
        row.edit = true
        isedit.value = true
        beforeIntroduction = row.introduction
    }
    else {
        ElNotification({
            message: '请先保存本次编辑',
            type: 'warning',
        })
    }
}

const handleSave = (index, row) => {
    row.edit = false
    isedit.value = false
    console.log(addcountry);

}

const handleConfirm = (index, row) => {
    //console.log(row)
    row.country = row.country.concat(addcountry.value)
    row.sex = row.sex.concat(addsex.value)
    row.nocountry = row.nocountry.filter(
        (item) => !addcountry.value.includes(item)
    )
    row.nosex = row.nosex.filter(
        (item) => !addsex.value.includes(item)
    )
    addcountry.value.splice(0)
    addsex.value.splice(0)
    console.log(props.items)
    emits('ModifyItem', row)
}

const handleCancel = (index, row) => {
    console.log(index, row)
    row.introduction = beforeIntroduction
    addcountry.value.splice(0)
    addsex.value.splice(0)
}

</script>