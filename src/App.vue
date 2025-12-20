<script setup>
import {ref, onBeforeMount} from "vue";
import rotations from "./rotation.json"

// 距离刷新地图的时间倒计时
let countdown = ref()
/**
 * 每秒计算倒计时
 */
onBeforeMount(() => {
    setInterval(() => {
        const date = new Date();
        // 获取当前小时
        const nowHours = date.getHours();
        // 获取当前分钟
        const nowMinutes = date.getMinutes();
        // 获取当前秒
        const nowSeconds = date.getSeconds();

        // 计算距离第二天凌晨还有多久
        // 计算小时
        const hours = (24 - nowHours).toString().padStart(2, "0");
        // 计算分钟
        const minutes = (59 - nowMinutes).toString().padStart(2, "0");
        // 计算秒
        const seconds = (59 - nowSeconds).toString().padStart(2, "0");
        countdown.value = `${hours}:${minutes}:${seconds}`;
    }, 1000);
})

const mapInfo = ref({})
onBeforeMount(() => {
    // 初始地图和时间的对应
    const initMap = {"date": new Date("2025/12/17 01:00:00"), "id": 16};
    // 获取当日时间
    const date = new Date();
    // 计算距离初始时间过去了多久
    const days = Math.floor((date.getTime() - initMap.date.getTime()) / (1000 * 60 * 60 * 24)) + 1;
    // 过去的天数加上轮转编号取模于21
    const rotationId = (days + initMap.id) % 21
    // 获取数据
    mapInfo.value = rotations[rotationId];
})
</script>

<template>
    <p>距离刷新地图还有: {{ countdown }}</p>
    <p>第一关: {{ mapInfo.level1.name }} {{ mapInfo.level1.remark }}</p>
    <p>第二关: {{ mapInfo.level2.name }} {{ mapInfo.level2.remark }}</p>
    <p>第三关: {{ mapInfo.level3.name }} {{ mapInfo.level3.remark }}</p>
    <p>第四关: 火山</p>
    <p>第五关: 熔炉</p>
    <p>第六关: 顶峰</p>
</template>

<style scoped>
</style>
