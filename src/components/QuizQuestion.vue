<template>
  <div class="q-enter">
    <div class="q-num">Q{{ current + 1 }} · {{ total }}</div>
    <div class="q-text">{{ question.q }}</div>
    <div class="opts">
      <button
        v-for="(opt, i) in question.opts"
        :key="i"
        class="opt"
        @click="$emit('pick', i)"
      >{{ opt.t }}</button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  question: { type: Object, required: true },
  current: { type: Number, required: true },
  total: { type: Number, required: true }
})
defineEmits(['pick'])
</script>
