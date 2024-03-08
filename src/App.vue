<script setup>
import { ref } from 'vue'
import TimeNow from './components/TimeNow.vue'
import RunArea from './components/RunArea.vue'
const numbers = ref(12)
</script>

<template>
  <input v-model="numbers">
  <TimeNow></TimeNow>
  <RunArea></RunArea>
  <Circle :percentagePar="numbers"></Circle>
</template>

<style scoped>
</style>
