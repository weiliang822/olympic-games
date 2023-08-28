<template>
    <h3>添加项目</h3>
    <el-form>
        <el-form-item label="项目名称">
            <el-input v-model="itemName" placeholder="请输入项目名称"></el-input>
        </el-form-item>
        <el-form-item label="参与国家">
            <el-select v-model="participatingCountries" placeholder="请选择参与国家（可搜索）" filterable multiple>
                <el-option v-for="(item, index) in props.countries" :key="index" :label=item :value=item></el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="性别">
            <el-select v-model="itemSex" placeholder="请选择项目性别">
                <el-option label="男" value="male" />
                <el-option label="女" value="female" />
            </el-select>
        </el-form-item>
        <el-form-item label="项目简介">
            <el-input placeholder="请输入项目简介" v-model="itemIntroduction" autosize type="textarea" />
        </el-form-item>
    </el-form>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
    countries: {
        type: Array,
        default: ['中国', '美国']
    }
})

const itemName = ref('')
const itemSex = ref('')
const itemIntroduction = ref('')
const participatingCountries = ref([])
</script>