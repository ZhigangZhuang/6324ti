<template>
  <div class="result">
    <div class="r-top">
      <div class="r-eyebrow">你的测试结果是</div>
      <div class="r-name">{{ result.name }}</div>
      <div class="r-alias">{{ result.alias }}</div>
    </div>
    <div class="r-tags">
      <span
        v-for="(tag, i) in result.tags"
        :key="i"
        class="r-tag"
        :class="{ hot: i === 0 }"
      >{{ tag }}</span>
    </div>
    <div class="r-divider"></div>
    <div class="r-card">{{ result.desc }}</div>
    <div class="r-quote">{{ result.quote }}</div>
    <button class="btn-again" @click="$emit('restart')">重开 / 再测一次</button>
  </div>
</template>

<script setup>
defineProps({
  result: { type: Object, required: true }
})
defineEmits(['restart'])
</script>
