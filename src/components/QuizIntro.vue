<template>
  <div class="intro">
    <button class="btn-start" @click="$emit('start')">开始测试</button>
    <p class="intro-notice">测试记录仅保存在当前浏览器，本网站不会上传你的答题数据。</p>
  </div>
</template>

<script setup>
defineEmits(['start'])
</script>
