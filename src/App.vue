<script setup>
import {ref, onBeforeMount} from "vue";
import rotations from "./rotation.json"

// 距离刷新地图的时间倒计时
const countdown = ref()
const nowDate = ref()
/**
 * 每秒计算倒计时
 */
onBeforeMount(() => {
    setInterval(() => {
        const date = new Date();
        // 获取当前年
        const nowYear = date.getFullYear();
        // 获取当前月
        const nowMonth = date.getMonth() + 1;
        // 获取当前日
        const nowDay = date.getDate();
        // 获取当前小时
        const nowHours = date.getHours();
        // 获取当前分钟
        const nowMinutes = date.getMinutes();
        // 获取当前秒
        const nowSeconds = date.getSeconds();
        nowDate.value = `${nowYear}/${nowMonth}/${nowDay}`;
        // 计算距离第二天凌晨还有多久
        // 计算小时
        const hours = (24 - nowHours).toString().padStart(2, "0");
        // 计算分钟
        const minutes = (59 - nowMinutes).toString().padStart(2, "0");
        // 计算秒
        const seconds = (59 - nowSeconds).toString().padStart(2, "0");
        // 设置倒计时时间
        countdown.value = `${hours}:${minutes}:${seconds}`;
        if (countdown.value !== "00:00:00") {
            calculateTodayMap()
        }
    }, 1000);
})

// 地图数据
const mapInfo = ref([])
/**
 * 计算当日是什么地图
 */
const calculateTodayMap = () => {
    // 初始地图和时间的对应
    const initMap = {"date": new Date("2025/12/17 01:00:00"), "id": 16};
    // 获取当日时间
    const date = new Date();
    // 计算距离初始时间过去了多久
    const days = Math.floor((date.getTime() - initMap.date.getTime()) / (1000 * 60 * 60 * 24)) + 1;
    // 过去的天数加上轮转编号取模于21, 计算出今天是第几轮
    const rotationId = (days + initMap.id) % 21
    // 获取数据
    mapInfo.value = rotations[rotationId];
}
/**
 * 在组件加载之前获取地图数据
 */
onBeforeMount(() => {
    // 获取地图数据
    calculateTodayMap()
})
</script>

<template>
    <p>
        <el-text>今日日期: {{ nowDate }}</el-text>
    </p>
    <p>
        <el-text>距离刷新地图还有:</el-text>
        <el-text type="success"> {{ countdown }}</el-text>
    </p>
    <el-table :data="mapInfo" border size="large">
        <el-table-column label="关卡" prop="id"></el-table-column>
        <el-table-column label="地形" prop="name"></el-table-column>
        <el-table-column label="备注" prop="remark"></el-table-column>
    </el-table>
</template>

<style scoped>
</style>
