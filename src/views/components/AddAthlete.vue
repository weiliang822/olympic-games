<template>
    <h3>添加运动员</h3>
    <el-form>
        <el-form-item label="国家">
            <el-select v-model="sel_country" placeholder="请选择国家" filterable>
                <el-option v-for="(item, index) in props.countries" :key="index" :label=index :value=index />
            </el-select>
        </el-form-item>
        <el-form-item label="项目">
            <el-select v-model="sel_items" placeholder="请选择参加的项目（可多选）" filterable multiple>
                <el-option v-for="(item, index) in props.countries[sel_country]" :key="index" :label=index :value=index />
            </el-select>
        </el-form-item>
        <el-form-item label="性别">
            <el-select v-model="sex" placeholder="请选择运动员性别">
                <el-option label="男" value="男" />
                <el-option label="女" value="女" />
            </el-select>
        </el-form-item>
        <el-form-item label="姓名">
            <el-input v-model="name" placeholder="请输入姓名"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submit">提交</el-button>
        </el-form-item>
    </el-form>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { ElNotification } from 'element-plus'

const props = defineProps({
    countries: {
        type: Object,
        default: {
            '中国': {
                '乒乓球': {
                    '男': {
                        '张继科': 98
                    },
                    '女': {
                        '陈梦': 99
                    }
                },
                '篮球': {
                    '男': {
                        '易建联': 98
                    }
                }
            }
        }
    }
})

const emits = defineEmits({
    UpdateAthlete: null
})

const sel_country = ref('')
const sel_items = ref([])
const sex = ref('')
const name = ref()

function submit() {
    if (sel_country.value && sel_items.value && sex.value && name.value) {
        emits('UpdateAthlete', {
            country: sel_country,
            items: sel_items,
            sex: sex,
            name: name
        })
    }
    else {
        ElNotification({
            message: '请完整填写表单',
            type: 'error',
        })
    }
}

</script>