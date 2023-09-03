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
                <el-menu-item index="1" @click="curTab = UpdateItems">项目管理</el-menu-item>
                <el-menu-item index="2" @click="curTab = UpdateGrades">成绩管理</el-menu-item>
                <el-menu-item index="4" @click="curTab = AddAthlete">添加运动员</el-menu-item>
                <el-menu-item index="5" @click="curTab = Overall">查询总分</el-menu-item>

            </el-menu>
            <el-footer style="position:fixed; bottom:10px">
                <el-row><el-text>author:2154057 汪清濯</el-text></el-row>
                <el-row><el-text>Copyright:2023 汪清濯</el-text></el-row>
                <el-row><el-text> <el-link href="https://github.com/weiliang822/olympic-games"
                            target="_blank">Github:weiliang822/olympic-games</el-link></el-text></el-row>
            </el-footer>
        </el-aside>

        <el-container style="width:75vw">
            <el-header>
                <el-row :gutter="20">
                    <el-tooltip effect="dark" content="点击查看帮助">
                        <el-button :icon="QuestionFilled" circle @click="HelpdialogVisible = true"></el-button>
                    </el-tooltip>
                    <!-- 帮助信息 -->
                    <el-dialog v-model="HelpdialogVisible" title="帮助">
                        <p v-for="item in Helpinfo">{{ item }}</p>
                        <template #footer>
                            <span class="dialog-footer">
                                <el-button type="primary" @click="HelpdialogVisible = false">
                                    确认
                                </el-button>
                            </span>
                        </template>
                    </el-dialog>
                </el-row>
            </el-header>
            <el-main>
                <KeepAlive>
                    <component :is="curTab" @ModifyItem="ModifyItem" @ModifyGrade="ModifyGrade"
                        @UpdateAthlete="UpdateAthlete" :items="items" :countries="countries" :tableData="tableData">
                    </component>
                </KeepAlive>
            </el-main>
        </el-container>
    </el-container>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'
import { MagicStick, QuestionFilled } from '@element-plus/icons-vue'
import { ElNotification } from 'element-plus'
import UpdateItems from './components/UpdateItems.vue'
import UpdateGrades from './components/UpdateGrades.vue'
import AddAthlete from './components/AddAthlete.vue'
import Overall from './components/Overall.vue'

const curTab = ref(Overall)

const HelpdialogVisible = ref(false)

//项目管理表格数据
const items = reactive([
    {
        name: '乒乓球',
        country: ['中国', '美国'],
        nocountry: ['英国', '法国'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '我觉得这是一种自信'
    },
    {
        name: '篮球',
        country: ['中国', '美国'],
        nocountry: ['英国', '法国'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '孩子们我回来了'
    }
    // {
    //     name: '肘击',
    //     country: ['俄罗斯', '美国'],
    //     nocountry: ['英国', '法国', '中国'],
    //     sex: ['男'],
    //     nosex: ['女'],
    //     introduction: '传奇机长佐巴扬'
    // }
])
//国家
const countries = reactive({
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
    },
    '美国': {
        '篮球': {
            '男': {
                '科比·布莱恩特·牢大': 824
            }
        },
        '乒乓球': {
            '男': {
            },
            '女': {
            }
        },
    },
    '英国': {},
    '法国': {},
    '俄罗斯': {}
})

//成绩管理表格数据
const tableData = reactive([
    {
        item: '乒乓球',
        country: '中国',
        sex: '男',
        name: '张继科',
        grade: 98
    },
    {
        item: '乒乓球',
        country: '中国',
        sex: '男',
        name: '马龙',
        grade: 100
    },
    {
        item: '篮球',
        country: '中国',
        sex: '男',
        name: '姚明',
        grade: 100
    },
    {
        item: '篮球',
        country: '中国',
        sex: '男',
        name: '易建联',
        grade: 95
    },
    {
        item: '篮球',
        country: '美国',
        sex: '男',
        name: '科比·布莱恩特·牢大',
        grade: 824
    },
    {
        item: '英雄联盟',
        country: '中国',
        sex: '男',
        name: '永远的神·乌兹',
        grade: 2800
    },
    {
        item: '英雄联盟',
        country: '韩国',
        sex: '男',
        name: '李相赫BigFly',
        grade: 9999
    },
    {
        item: '英雄联盟',
        country: '中国',
        sex: '男',
        name: '销户',
        grade: 2200
    },
])

//国家
const CountryMedals = reactive({
    '中国': {
        '总奖牌': 63,
        '总积分': 642,
        '金牌': 28,
        '银牌': 15,
        '铜牌': 20
    }
})

//个人奖牌
const IndividualMedals = reactive({
    '张继科': {
        '金': 1,
        '银': 0,
        '铜': 0
    }
})

//添加运动员
function UpdateAthlete(msg) {
    console.log(msg.country.value)
    // 数组用for..of循环
    let athcountry = msg.country.value;
    let athitems = msg.items.value;
    let athsex = msg.sex.value;
    let athname = msg.name.value;
    for (let athitem of athitems) {
        console.log(athitem)
        countries[athcountry][athitem][athsex][athname] = 0
        tableData.push({
            item: athitem,
            country: athcountry,
            sex: athsex,
            name: athname,
            grade: 0
        })
    }
    //console.log(countries)
}

//项目修改 
function ModifyItem(msg) {
    console.log(msg)
    for (let country of msg.country) {
        if (countries[country][msg.name] == undefined) {
            countries[country][msg.name] = {}
        }
        for (let sex of msg.sex)
            if (countries[country][msg.name][sex] == undefined) {
                countries[country][msg.name][sex] = {}
            }
    }
    console.log(countries)
}

//成绩修改
function ModifyGrade(msg) {
    //console.log(tableData)
    countries[msg.country][msg.item][msg.sex][msg.name] = msg.grade
}


</script>

<style scoped></style>
