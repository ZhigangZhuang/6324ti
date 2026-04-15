<template>
  <div class="progress">
    <div class="prog-row">
      <span>{{ done ? '完成！' : `第 ${current + 1} 题 / 共 ${total} 题` }}</span>
      <span>{{ pct }}%</span>
    </div>
    <div class="prog-track">
      <div class="prog-fill" :style="{ width: pct + '%' }"></div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  current: { type: Number, required: true },
  total: { type: Number, required: true },
  done: { type: Boolean, default: false }
})

const pct = computed(() =>
  props.done ? 100 : Math.round(props.current / props.total * 100)
)
</script>
