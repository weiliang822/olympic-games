<template>
    <h3>总榜</h3>
    <h4>总奖牌榜</h4>
    <div id="TotalMedalChart" style="width: 70%;height: 50%;"></div>
    <h4>总积分榜</h4>
    <div id="TotalPointChart" style="width: 70%;height: 50%;"></div>
    <h4>金牌榜</h4>
    <div id="GoldMedalChart" style="width: 70%;height: 50%;"></div>
    <h4>银牌榜</h4>
    <div id="SilverMedalChart" style="width: 70%;height: 50%;"></div>
    <h4>铜牌榜</h4>
    <div id="BronzeMedalChart" style="width: 70%;height: 50%;"></div>
    <h4>男总积分榜</h4>
    <div id="MalePointChart" style="width: 70%;height: 50%;"></div>
    <h4>女总积分榜</h4>
    <div id="FemalePointChart" style="width: 70%;height: 50%;"></div>
</template>

<script setup>
import * as echarts from 'echarts'
import { ref, reactive, computed, onMounted, watch } from 'vue'
const props = defineProps({
    items: {
        type: Object
    },
    countries: {
        type: Object
    },
})

onMounted(() => {
    //console.log(gradeData.value, golddata.value, ydata.value);
    initcharts()
})

const points = [7, 5, 3, 2, 1] // 各项目前五名的加分

const gradeData = computed(() => {
    let tempdata = {}
    for (let item of props.items) {
        let tempmalearr = []
        let tempfemalearr = []  // 暂存此项目男女的成绩
        for (let country of item.country) {
            if (tempdata[country] == undefined)
                tempdata[country] = {
                    '总奖牌': 0,
                    '总积分': 0,
                    '男总积分': 0,
                    '女总积分': 0,
                    '金牌': 0,
                    '银牌': 0,
                    '铜牌': 0
                }
            if (props.countries[country][item.name]['男'] != undefined) {
                for (let athlete in props.countries[country][item.name]['男']) {
                    //tempmalearr.push({ country: props.countries[country][item.name]['男'][athlete] })不行，键名会是country
                    tempmalearr.push({})
                    tempmalearr[tempmalearr.length - 1][country] = props.countries[country][item.name]['男'][athlete]
                }
            }
            if (props.countries[country][item.name]['女'] != undefined) {
                for (let athlete in props.countries[country][item.name]['女']) {
                    tempfemalearr.push({})
                    tempfemalearr[tempfemalearr.length - 1][country] = props.countries[country][item.name]['女'][athlete]
                }
            }
            function gradesort(a, b) {
                return b[Object.keys(b)[0]] - a[Object.keys(a)[0]]
            }
            //把每个项目的成绩从高到低排序
            tempmalearr.sort(gradesort)
            tempfemalearr.sort(gradesort)
            //console.log(tempmalearr, tempfemalearr)
        }
        //计算总分
        for (let i = 0; i < Math.min(5, tempmalearr.length); i++) {
            //成绩不为0有效
            let nowcountry = Object.keys(tempmalearr[i])[0]
            if (tempmalearr[i][nowcountry] != 0) {
                if (i == 0)
                    tempdata[nowcountry]['金牌']++
                else if (i == 1)
                    tempdata[nowcountry]['银牌']++
                else if (i == 2)
                    tempdata[nowcountry]['铜牌']++
                if (i < 3)
                    tempdata[nowcountry]['总奖牌']++
                tempdata[nowcountry]['总积分'] += points[i];
                tempdata[nowcountry]['男总积分'] += points[i];
            }
        }
        for (let i = 0; i < Math.min(5, tempfemalearr.length); i++) {
            //成绩不为0有效
            let nowcountry = Object.keys(tempfemalearr[i])[0]
            if (tempfemalearr[i][nowcountry] != 0) {
                if (i == 0)
                    tempdata[nowcountry]['金牌']++
                else if (i == 1)
                    tempdata[nowcountry]['银牌']++
                else if (i == 2)
                    tempdata[nowcountry]['铜牌']++
                if (i < 3)
                    tempdata[nowcountry]['总奖牌']++
                tempdata[nowcountry]['总积分'] += points[i];
                tempdata[nowcountry]['女总积分'] += points[i];
            }
        }
    }
    return tempdata
}, { immediate: true, deep: true })

//以下数据设置echarts的options所用
const ydata = computed(
    () => Object.keys(gradeData.value)
)

const golddata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['金牌'])
    }
    return tmp
})

const silverdata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['银牌'])
    }
    return tmp
})

const bronzedata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['铜牌'])
    }
    return tmp
})

const pointdata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['总积分'])
    }
    return tmp
})

const malepointdata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['男总积分'])
    }
    return tmp
})

const femalepointdata = computed(() => {
    let tmp = []
    for (let key in gradeData.value) {
        tmp.push(gradeData.value[key]['女总积分'])
    }
    return tmp
})

const baseoption = {
    title: {
        text: ''
    },
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow'
        }
    },
    legend: {},
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
    },
    xAxis: {
        type: 'value',
        //boundaryGap: [0, 0.01]
    },
    yAxis: {
        type: 'category',
        inverse: true,
        data: []
    },
    series: []
}

const TotalMedalOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '总奖牌榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '金牌',
        type: 'bar',
        data: golddata.value
    })
    baseoption.series.push({
        realtimeSort: true,
        name: '银牌',
        type: 'bar',
        data: silverdata.value
    })
    baseoption.series.push({
        realtimeSort: true,
        name: '铜牌',
        type: 'bar',
        data: bronzedata.value
    })
    return baseoption
})

const GoldMedalOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '金牌榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '金牌',
        type: 'bar',
        data: golddata.value
    })
    return baseoption
})

const SilverMedalOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '银牌榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '银牌',
        type: 'bar',
        data: silverdata.value
    })
    return baseoption
})

const BronzeMedalOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '铜牌榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '铜牌',
        type: 'bar',
        data: bronzedata.value
    })
    return baseoption
})

const TotalPointOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '总积分榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '总积分',
        type: 'bar',
        data: pointdata.value
    })
    return baseoption
})

const MalePointOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '男总积分榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '男总积分',
        type: 'bar',
        data: malepointdata.value
    })
    return baseoption
})

const FemalePointOption = computed(() => {
    baseoption.series = []
    baseoption.title.text = '女总积分榜'
    baseoption.yAxis.data = ydata.value
    baseoption.series.push({
        realtimeSort: true,
        name: '女总积分',
        type: 'bar',
        data: femalepointdata.value
    })
    return baseoption
})

let TotalMedalChart, TotalPointChart, GoldMedalChart, SilverMedalChart, BronzeMedalChart, MalePointChart, FemalePointChart

function initcharts() {
    TotalMedalChart = echarts.init(document.getElementById('TotalMedalChart'))
    TotalPointChart = echarts.init(document.getElementById('TotalPointChart'))
    GoldMedalChart = echarts.init(document.getElementById('GoldMedalChart'))
    SilverMedalChart = echarts.init(document.getElementById('SilverMedalChart'))
    BronzeMedalChart = echarts.init(document.getElementById('BronzeMedalChart'))
    MalePointChart = echarts.init(document.getElementById('MalePointChart'))
    FemalePointChart = echarts.init(document.getElementById('FemalePointChart'))
    TotalMedalChart.setOption(TotalMedalOption.value)
    GoldMedalChart.setOption(GoldMedalOption.value)
    SilverMedalChart.setOption(SilverMedalOption.value)
    BronzeMedalChart.setOption(BronzeMedalOption.value)
    TotalPointChart.setOption(TotalPointOption.value)
    MalePointChart.setOption(MalePointOption.value)
    FemalePointChart.setOption(FemalePointOption.value)
}

watch(gradeData, () => {
    TotalMedalChart.setOption(TotalMedalOption.value)
    GoldMedalChart.setOption(GoldMedalOption.value)
    SilverMedalChart.setOption(SilverMedalOption.value)
    BronzeMedalChart.setOption(BronzeMedalOption.value)
    TotalPointChart.setOption(TotalPointOption.value)
    MalePointChart.setOption(MalePointOption.value)
    FemalePointChart.setOption(FemalePointOption.value)
})

</script>