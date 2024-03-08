<script setup>
import { ref, onUnmounted } from 'vue'

// 存储运动时间
const runTimeCount = ref(50)

const updateTimeCount = () => {
    if (runTimeCount.value === 0) {
        runTimeCount.value = 50
    } else {
        runTimeCount.value--
    }
}


let timer = null
//开始计时
const startUpDateTimeCount = () => {
    timer = setInterval(() => {
        updateTimeCount()
    }, 1000)
}

// 暂停计时
const pauseTimeCount = () => {
    clearInterval(timer)
}

// 重新开始
const restartTimeCount = () => {
    clearInterval(timer) // 清除当前的定时器
    runTimeCount.value = 50
    startUpDateTimeCount()
}

// 组件销毁时清除定时器
onUnmounted(() => {
    clearInterval(timer)
})

</script>

<template>
    <div>
        <div class="time_area">当前次数:
            <span>{{ runTimeCount }}次</span>

            <button @click="startUpDateTimeCount">开始运动</button>
            <button @click="pauseTimeCount">暂停运动</button>
            <button @click="restartTimeCount">重新开始</button>
        </div>
    </div>
</template>

<style scoped>
.time_area {
    display: flex;
    justify-content: left;
}
</style>