<template>
    <el-container class="layout-container-demo" style="height: 95vh">
        <el-aside style="width:25vw">
            <h1 class="mb-2" style="margin-left: 1vw;">
                奥运会
            </h1>
            <el-menu :default-openeds="['1', '4']">
                <!-- <template #title>
                    Select sort method
                </template> -->
                <el-menu-item index="1" @click="">输入、修改成绩</el-menu-item>
                <el-menu-item index="2" @click="">查询成绩</el-menu-item>
                <el-menu-item index="3" @click="">添加项目</el-menu-item>
                <el-menu-item index="4" @click="">添加运动员</el-menu-item>
                <el-menu-item index="5" @click="">查询总分</el-menu-item>
                <!-- <el-menu-item index="6" @click="">Heap Sort</el-menu-item>
                <el-menu-item index="7" @click="">Merge Sort</el-menu-item> -->

            </el-menu>
            <el-footer style="position:fixed; bottom:10px">
                <el-row><el-text>author:2154057 汪清濯</el-text></el-row>
                <el-row><el-text>Copyright:2023 汪清濯</el-text></el-row>
                <el-row><el-text> <el-link href="" target="_blank">Github:</el-link></el-text></el-row>
            </el-footer>
        </el-aside>

        <el-container style="width:75vw">
            <el-header>
                <el-row :gutter="20">
                    <el-tooltip effect="dark" content="click to get help">
                        <el-button :icon="QuestionFilled" circle @click="HelpdialogVisible = true"></el-button>
                    </el-tooltip>
                    <!-- 帮助信息 -->
                    <el-dialog v-model="HelpdialogVisible" title="Help">
                        <p v-for="item in Helpinfo">{{ item }}</p>
                        <template #footer>
                            <span class="dialog-footer">
                                <el-button type="primary" @click="HelpdialogVisible = false">
                                    Confirm
                                </el-button>
                            </span>
                        </template>
                    </el-dialog>
                </el-row>
            </el-header>
            <el-main>
                <AddItem />
                <QueryGrades />
                <InputGrades />
                <AddAthlete />
                <Overall />
            </el-main>
        </el-container>
    </el-container>
</template>

<script setup>
import { ref, watch } from 'vue'
import { QuestionFilled } from '@element-plus/icons-vue'
import { ElNotification } from 'element-plus'
import AddItem from './components/AddItem.vue'
import QueryGrades from './components/QueryGrades.vue'
import InputGrades from './components/InputGrades.vue'
import AddAthlete from './components/AddAthlete.vue'
import Overall from './components/Overall.vue'

const props = defineProps({
    data: {

    }
})
const HelpdialogVisible = ref(false)


</script>

<style scoped></style>
