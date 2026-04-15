<template>
  <div id="app">
    <AppHeader />

    <QuizIntro v-if="phase === 'intro'" @start="startQuiz" />

    <QuizProgress
      v-if="phase !== 'intro'"
      :current="cur"
      :total="DRAW"
      :done="phase === 'result'"
    />

    <QuizQuestion
      v-if="phase === 'quiz'"
      :key="cur"
      :question="qs[cur]"
      :current="cur"
      :total="DRAW"
      @pick="pick"
    />

    <QuizResult
      v-if="phase === 'result'"
      :result="R[winner]"
      @restart="restart"
    />
  </div>
</template>

<script setup>
import { ref, reactive, nextTick } from 'vue'
import AppHeader from './components/AppHeader.vue'
import QuizIntro from './components/QuizIntro.vue'
import QuizProgress from './components/QuizProgress.vue'
import QuizQuestion from './components/QuizQuestion.vue'
import QuizResult from './components/QuizResult.vue'
import { R } from './data/results.js'
import { POOL } from './data/questions.js'

const DRAW = 16

const phase = ref('intro')
const scores = reactive({})
const qs = ref([])
const cur = ref(0)
const winner = ref(null)

function shuffle(a) {
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]]
  }
  return a
}

function startQuiz() {
  Object.keys(R).forEach(k => scores[k] = 0)
  qs.value = shuffle([...POOL]).slice(0, DRAW)
  cur.value = 0
  phase.value = 'quiz'
  nextTick(() => window.scrollTo({ top: 0, behavior: 'smooth' }))
}

function pick(i) {
  const o = qs.value[cur.value].opts[i]
  Object.entries(o.sc).forEach(([k, v]) => {
    if (scores[k] !== undefined) scores[k] += v
  })
  cur.value++
  if (cur.value >= DRAW) {
    showResult()
  } else {
    nextTick(() => window.scrollTo({ top: 0, behavior: 'smooth' }))
  }
}

function showResult() {
  const appearances = {}
  Object.keys(R).forEach(k => appearances[k] = 0)
  qs.value.forEach(q =>
    q.opts.forEach(o =>
      Object.keys(o.sc).forEach(id => {
        if (appearances[id] !== undefined) appearances[id]++
      })
    )
  )
  const normalized = {}
  Object.entries(scores).forEach(([id, score]) => {
    normalized[id] = appearances[id] > 0 ? score / appearances[id] : 0
  })
  winner.value = Object.entries(normalized).sort((a, b) => b[1] - a[1])[0][0]
  phase.value = 'result'
  nextTick(() => window.scrollTo({ top: 0, behavior: 'smooth' }))
}

function restart() {
  Object.keys(R).forEach(k => scores[k] = 0)
  winner.value = null
  cur.value = 0
  phase.value = 'intro'
  nextTick(() => window.scrollTo({ top: 0, behavior: 'smooth' }))
}
</script>
