<script setup>
import { ref, onUnmounted, computed, watch } from 'vue';

// 存储运动时间
const runTimeCount = ref(50);

const updateTimeCount = () => {
  if (runTimeCount.value === 0) {
    runTimeCount.value = 50;
  } else {
    runTimeCount.value--;
  }
};

let timer = null;
//开始计时
const startUpDateTimeCount = () => {

  timer = setInterval(() => {
    updateTimeCount();
  }, 1000);
};

// 暂停计时
const pauseTimeCount = () => {
  clearInterval(timer);
};

// 重新开始
const restartTimeCount = () => {
  clearInterval(timer); // 清除当前的定时器
  runTimeCount.value = 50;
  startUpDateTimeCount();
};

// 播放提示音
const fiveseconed = ref(null);
const nextmix = ref(null);

// 组件销毁时清除定时器
onUnmounted(() => {
  clearInterval(timer);
});

// 计算百分比
const percentage = computed(() => {
  return ((50 - runTimeCount.value) / 50) * 100;
});

// 监听进度条
watch(runTimeCount, newRunTimeCount => {
  if (newRunTimeCount === 26) {
    fiveseconed.value.play();
  }
  if (newRunTimeCount === 5) {
    nextmix.value.play();
  }
});

const duration = computed(() => Math.floor(percentage / 10));
const colors = [
  { color: '#df3451', percentage: 60 },
  { color: '#5cb85c', percentage: 100 },
];
</script>

<template>
  <div>
    <audio ref="fiveseconed">
      <source
        src="../assets/fivesecond.mp3"
        type="audio/mpeg"
      />
    </audio>
    <audio ref="nextmix">
      <source
        src="../assets/nextmix.mp3"
        type="audio/mpeg"
      />
    </audio>
    <div class="time_area">
      <div>剩余时间</div>

      <el-progress
        class="time_progress"
        type="line"
        :percentage="percentage"
        striped
        :show-text="false"
        :stroke-width="30"
        :color="colors"
      />

      <span>{{ runTimeCount }}秒</span>
      <div class="button_area">
        <el-button
          type="primary"
          @click="startUpDateTimeCount"
          >开始运动</el-button
        >
        <el-button
          type="danger"
          @click="pauseTimeCount"
          >暂停运动</el-button
        >
        <el-button
          type="success"
          @click="restartTimeCount"
          >重新开始</el-button
        >
      </div>
    </div>
  </div>
</template>

<style scoped>
.time_area {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 500px;
}

.button_area {
  padding-top: 30px;
}

.time_progress {
  margin-top: 30px;
  max-width: 500px;
  width: 90vw;
}
</style>
