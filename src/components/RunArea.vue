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
  isStart.value = true;
  beginStart.value.play();
  timer = setInterval(() => {
    updateTimeCount();
  }, 1000);
};

// 暂停计时
const pauseTimeCount = () => {
  pauseRun.value.play();
  isStart.value = false;
  clearInterval(timer);
};

// 重新开始
const restartTimeCount = () => {
  isStart.value = false;
  clearInterval(timer); // 清除当前的定时器
  runTimeCount.value = 50;
  startUpDateTimeCount();
};

// 播放提示音
const fiveseconed = ref(null);
const nextmix = ref(null);
const beginStart = ref(null);
const pauseRun = ref(null);

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

// 展示在页面上的数据
const showNumber = computed(() => {
  // 如果 runTimeCount >= 20 返回 50-20
  if (runTimeCount.value >= 20) {
    return runTimeCount.value - 20;
  } else {
    return runTimeCount.value;
  }
});

const colors = [
  { color: '#df3451', percentage: 60 },
  { color: '#5cb85c', percentage: 100 },
];

const isStart = ref(false);
</script>

<template>
  <div>
    <audio ref="beginStart">
      <source
        src="../assets/start.aac"
        type="audio/mpeg"
      />
    </audio>
    <audio ref="pauseRun">
      <source
        src="../assets/pause.mp3"
        type="audio/mpeg"
      />
    </audio>
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
      <h3>HIT 燃脂计时器</h3>
      <el-progress
        class="time_progress"
        type="line"
        :percentage="percentage"
        striped
        :show-text="false"
        :stroke-width="30"
        :color="colors"
      />

      <h4>{{ runTimeCount >= 20 ? `运动剩余` : `休息还剩` }}</h4>
      <h3>
        <span
          :style="{
            color: runTimeCount >= 20 ? '#df3451' : '#5cb85c',
            fontSize: '5em',
          }"
        >
          {{ showNumber }}
        </span>
        <span>秒</span>
      </h3>

      <div class="button_area">
        <el-button
          v-if="!isStart"
          type="primary"
          @click="startUpDateTimeCount"
          >开始运动</el-button
        >
        <el-button
          v-else
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
