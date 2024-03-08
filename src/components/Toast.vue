<script setup>
import { reactive, onUnmounted } from 'vue';

</script>

<template>
  <div
    class="my_toast"
  >
    自定义弹窗
  </div>
</template>

<style scoped>
.my_toast {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 99999;
  border: 1px solid;
  padding: 5px;
  border-radius: 5px;
  box-shadow: 0px 0px 2px 1px #b9b9b9;
}
</style>
