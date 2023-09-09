<template>
    <div class="bgimage"></div>
    <el-container class="layout-container-demo" style="height: 95vh">
        <el-aside style="width:25vw">
            <h1 class="mb-2" style="margin-left: 10vw;color: rgb(0, 0, 0);">
                奥运会
            </h1>
            <el-menu :default-openeds="['1', '4']">
                <!-- <template #title>
                    Select sort method
                </template> -->
                <el-menu-item index="1" @click="curTab = UpdateItems">项目管理</el-menu-item>
                <el-menu-item index="2" @click="curTab = UpdateGrades">成绩管理</el-menu-item>
                <el-menu-item index="3" @click="curTab = AddAthlete">添加运动员</el-menu-item>
                <el-menu-item index="4" @click="curTab = Overall">查询总分</el-menu-item>

            </el-menu>
            <el-footer style="position:fixed; bottom:10px">
                <el-row><el-text>author:2154057 汪清濯</el-text></el-row>
                <el-row><el-text>Copyright © 2023-present 汪清濯</el-text></el-row>
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
const allcountry = ['中国', '美国', '英国', '法国', '俄罗斯', '日本', '澳大利亚', '意大利', '德国', '荷兰',
    '加拿大', '巴西', '新西兰', '匈牙利', '韩国', '乌克兰', '西班牙', '古巴', '波兰', '瑞士', '土耳其', '牙买加']

//项目管理表格数据
const items = reactive([
    {
        name: '乒乓球',
        country: ['中国', '日本', '德国', '韩国', '荷兰', '美国', '巴西'],
        nocountry: ['英国', '法国', '俄罗斯', '澳大利亚', '意大利', '加拿大', '新西兰',
            '匈牙利', '乌克兰', '西班牙', '古巴', '波兰', '瑞士', '土耳其', '牙买加'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '我觉得这是一种自信'
    },
    {
        name: '100m',
        country: ['中国', '美国', '意大利', '加拿大', '牙买加', '瑞士'],
        nocountry: ['英国', '法国', '俄罗斯', '日本', '澳大利亚', '德国', '荷兰',
            '巴西', '新西兰', '匈牙利', '韩国', '乌克兰', '西班牙', '古巴', '波兰', '土耳其'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '苏神牛逼'
    },
    {
        name: '100m自由泳',
        country: ['美国', '意大利', '韩国', '法国', '澳大利亚', '加拿大', '荷兰', '英国'],
        nocountry: ['中国', '俄罗斯', '日本', '德国', '瑞士', '巴西', '新西兰', '匈牙利',
            '韩国', '乌克兰', '西班牙', '古巴', '波兰', '土耳其', '牙买加'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '100m自由泳'
    },
    {
        name: '跳水',
        country: ['中国', '英国', '俄罗斯', '美国', '澳大利亚'],
        nocountry: ['日本', '德国', '瑞士', '巴西', '新西兰', '匈牙利', '意大利', '韩国', '法国', '加拿大', '荷兰',
            '韩国', '乌克兰', '西班牙', '古巴', '波兰', '土耳其', '牙买加'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '跳水10米台'
    },
    {
        name: '网球',
        country: ['德国', '西班牙', '俄罗斯', '法国', '瑞士', '乌克兰', '意大利'],
        nocountry: ['日本', '中国', '巴西', '新西兰', '匈牙利', '韩国', '加拿大', '荷兰',
            '韩国', '古巴', '波兰', '土耳其', '牙买加', '澳大利亚', '英国', '美国'],
        sex: ['男', '女'],
        nosex: [],
        introduction: '网球'
    }
])
//国家
const countries = reactive({
    '中国': {
        '乒乓球': {
            '男': {
                '马龙': 100,
                '樊振东': 99
            },
            '女': {
                '陈梦': 100,
                '孙颖莎': 99
            }
        },
        '100m': {
            '男': {
                '苏炳添': 96
            }
        },
        '跳水': {
            '男': {
                '曹源': 100,
                '杨建': 99
            },
            '女': {
                '全红婵': 100,
                '陈雨熙': 99
            }
        }
    },
    '美国': {
        '乒乓球': {
            '女': {
                'Juan Liu': 93
            }
        },
        '100m': {
            '男': {
                'Fred KERLEY': 99,
                'Ronnie BAKER': 97
            },
        },
        '100m自由泳': {
            '男': {
                'Caeleb DRESSEL': 100
            }
        },
        '跳水': {
            '女': {
                'Delaney SCHNELL': 97
            }
        }
    },
    '英国': {
        '100m自由泳': {
            '女': {
                'Anna HOPKIN': 98,
            }
        },
        '跳水': {
            '男': {
                'Tom DALEY': 98
            },
            '女': {
                'Andrea SPENDOLINI SIRIEIX': 96
            }
        }
    },
    '法国': {
        '100m自由泳': {
            '男': {
                'Maxime GROUSSET': 98
            }
        },
        '网球': {
            '男': {
                'Jeremy CHARDY': 97,
                'Ugo HUMBERT': 96
            }
        }
    },
    '俄罗斯': {
        '跳水': {
            '男': {
                'Aleksandr BONDAR': 97,
                'Viktor MINIBAEV': 96
            }
        },
        '网球': {
            '男': {
                'Karen KHACHANOV': 99
            }
        }
    },
    '日本': {
        '乒乓球': {
            '女': {
                'Mima Ito': 98,
                'Kasumi ISHIKAWA': 96
            }
        }
    },
    '澳大利亚': {
        '100m自由泳': {
            '男': {
                'Kyle CHALMERS': 99
            },
            '女': {
                'Emma MCKEON': 100,
                'Cate CAMPBELL': 99
            }
        },
        '跳水': {
            '女': {
                'Melissa WU': 98
            }
        }
    },
    '意大利': {
        '100m': {
            '男': {
                'Lamont Marcell JACOBS': 100
            },
        },
        '100m自由泳': {
            '男': {
                'Alessandro MIRESSI': 96
            }
        },
        '网球': {
            '女': {
                'Camila GIORGI': 97
            }
        }
    },
    '德国': {
        '乒乓球': {
            '男': {
                'Dimitrij OVTCHAROV': 98
            },
            '女': {
                'Ying Han': 97
            }
        },
        '网球': {
            '男': {
                'Alexander ZVEREV': 100
            }
        }
    },
    '荷兰': {
        '乒乓球': {
            '女': {
                'Britt Eerland': 94
            }
        },
        '100m自由泳': {
            '女': {
                'Femke HEEMSKERK': 98,
            }
        }
    },
    '加拿大': {
        '100m': {
            '男': {
                'Andre DE GRASSE': 98
            },
        },
        '100m自由泳': {
            '女': {
                'Penny OLEKSIAK': 98,
            }
        }
    },
    '巴西': {
        '乒乓球': {
            '男': {
                'Hugo CALDERANO': 97
            }
        }
    },
    '新西兰': {},
    '匈牙利': {},
    '韩国': {
        '乒乓球': {
            '男': {
                'Hugo CALDERANO': 96
            },
            '女': {
                'Jihee Jeon': 95
            }
        },
        '100m自由泳': {
            '男': {
                'Sunwoo HWANG': 97
            }
        }
    },
    '乌克兰': {
        '网球': {
            '女': {
                'Elina SVITOLINA': 99
            }
        }
    },
    '西班牙': {
        '网球': {
            '男': {
                'Pablo CARRENO BUSTA': 98
            },
            '女': {
                'Paula BADOSA': 98,
                'Garbine MUGURUZA': 96
            }
        }
    },
    '古巴': {},
    '波兰': {},
    '瑞士': {
        '100m': {
            '女': {
                'Ajla DEL PONTE': 97,
                'Mujinga KAMBUNDJI': 96
            }
        },
        '网球': {
            '女': {
                'Belinda BENCIC': 100
            }
        }
    },
    '土耳其': {},
    '牙买加': {
        '100m': {
            '女': {
                '伊莱恩·汤普森-赫拉': 100,
                'Shelly-Ann FRASER-PRYCE': 99,
                'Shericka JACKSON': 98
            }
        }
    }
})



//成绩管理表格数据
const tableData = computed(() => {
    let tmp = []
    for (let curcountry in countries) {
        for (let curitem in countries[curcountry]) {
            for (let cursex in countries[curcountry][curitem]) {
                for (let athlete in countries[curcountry][curitem][cursex]) {
                    tmp.push({
                        item: curitem,
                        country: curcountry,
                        sex: cursex,
                        name: athlete,
                        grade: countries[curcountry][curitem][cursex][athlete]
                    })
                }
            }
        }
    }
    return tmp
})

const tableDatademo = reactive([
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

const Helpinfo = [
    '这是一个奥运会管理项目,你可以增添或修改奥运会项目,注册运动员,修改运动员成绩,并查看最后的总榜',
    '要注意,为某位运动员报名某项目时,该项目必须已经注册过并且添加了该运动员的国家,否则请先去注册项目',
    '成绩管理中的成绩全部换算为百分制,便于排序',
    '运动员或项目一经注册无法删除,运动员只能够修改成绩,项目只能增加参与的国家和性别,以及修改简介',
    '本项目初始数据来自部分2020年东京奥运会数据',
    '感谢您的支持!'
]


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

<style scoped>
.bgimage {
    background-image: url("../assets/VCG211300690892.jpg");
    background-size: cover;
    position: absolute;
    height: 98vh;
    width: 98vw;
    opacity: 0.3;
}

.el-menu {
    background-color: transparent !important;
}
</style>
