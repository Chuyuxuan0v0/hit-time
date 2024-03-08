<script setup>
import { ref, onUnmounted } from 'vue'

// 存储当前时间
const currentTime = ref('')
const milliseconds = ref(0)

// 获取当前时间并更新
const updateTime = () => {
    const now = new Date()
    const hours = padZero(now.getHours())
    const minutes = padZero(now.getMinutes())
    const seconds = padZero(now.getSeconds())
    milliseconds.value = padZero(now.getMilliseconds(), 3) // 保证毫秒数显示为三位数
    currentTime.value = `${hours}:${minutes}:${seconds}`
}

// 每秒更新一次时间
const timer = setInterval(() => {
    updateTime()
}, 1) // 更新间隔为1毫秒

// 组件销毁时清除定时器
onUnmounted(() => {
    clearInterval(timer)
})

// 辅助函数：确保数字始终以指定长度显示，并在不足时前面补0
const padZero = (value, length = 2) => {
    return value.toString().padStart(length, '0')
}
</script>

<template>
    <div>
        <div class="time_area">当前时间: 
            <span>{{ `${currentTime.split(":")[0]}` }}时</span>
            <span>{{ `${currentTime.split(":")[1]}` }}分</span>
            <span>{{ `${currentTime.split(":")[2]}` }}秒</span>
        
        </div>
    </div>
</template>

<style scoped>
.time_area{
    display: flex;
    justify-content: left;
}
</style>