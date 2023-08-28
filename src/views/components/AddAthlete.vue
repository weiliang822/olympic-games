<template>
    <h3>添加运动员</h3>
    <el-form>
        <el-form-item label="国家">
            <el-select v-model="sel_country" placeholder="请选择国家" filterable>
                <el-option v-for="(item, index) in countries" :key="index" :label=item :value=item />
            </el-select>
        </el-form-item>
        <el-form-item label="项目">
            <el-select v-model="sel_item" placeholder="请选择参加的项目（可多选）" filterable multiple>
                <el-option v-for="(item, index) in props.counry_item[sel_country]" :key="index" :label=item :value=item />
            </el-select>
        </el-form-item>
        <el-form-item label="性别">
            <el-select v-model="sex" placeholder="请选择项目性别">
                <el-option label="男" value="male" />
                <el-option label="女" value="female" />
            </el-select>
        </el-form-item>
        <el-form-item label="姓名">
            <el-input v-model="name" placeholder="请输入姓名"></el-input>
        </el-form-item>
    </el-form>
</template>

<script setup>
import { ref, reactive } from 'vue'

const props = defineProps({
    counry_item: {
        type: Object,
        default: {
            '中国': ['乒乓球', '游泳'],
            '美国': ['篮球', '游泳']
        }
    },
    countries: {
        type: Array,
        default: ['中国', '美国']
    }
})

const sel_country = ref('')
const sel_item = ref('')
const sex = ref('')
const sel_athlete = ref('')
const name = ref()

</script>