<template>
  <div class="intro">
    <button class="btn-start" @click="$emit('start')">开始测试</button>
  </div>
</template>

<script setup>
defineEmits(['start'])
</script>
