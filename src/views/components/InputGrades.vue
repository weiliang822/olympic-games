<template>
    <h3>输入、修改成绩</h3>
    <el-form>
        <el-form-item label="国家">
            <el-select v-model="sel_country" placeholder="请选择国家" filterable>
                <el-option v-for="(item, index) in countries" :key="index" :label=item :value=item />
            </el-select>
        </el-form-item>
        <el-form-item label="项目">
            <el-select v-model="sel_item" placeholder="请选择项目" filterable>
                <el-option v-for="(item, index) in props.counry_item[sel_country]" :key="index" :label=item :value=item />
            </el-select>
        </el-form-item>
        <el-form-item label="性别">
            <el-select v-model="itemSex" placeholder="请选择项目性别">
                <el-option label="男" value="male" />
                <el-option label="女" value="female" />
            </el-select>
        </el-form-item>
        <el-form-item label="运动员">
            <el-select v-model="sel_athlete" placeholder="请选择运动员" filterable>
                <el-option v-for="(item, index) in props.counry_item[sel_country]" :key="index" :label=item :value=item />
            </el-select>
        </el-form-item>
        <el-form-item label="输入成绩">
            <el-input v-model="grade" placeholder="请输入成绩"></el-input>
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
const itemSex = ref('')
const sel_athlete = ref('')
const grade = ref()

</script>