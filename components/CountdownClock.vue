<script setup lang="ts">
const duration = ref(60 * 0.05)
const time = ref({ minutes: 25, seconds: 0 })

const countdown = () => {
  time.value.minutes = Math.floor(duration.value / 60)
  time.value.seconds = Math.floor(duration.value % 60)
  --duration.value
}

const { pause, resume, isActive } = useIntervalFn(() => {
  countdown()
}, 1000, { immediate: false })

const restart = () => {
  pause()
  duration.value = 60 * 25
  time.value.minutes = 25
}

watch(duration, (v) => {
  if (v < 0)
    restart()
})
</script>

<template>
  <div class="text-center">
    <p>{{ time.minutes.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}:{{ time.seconds.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}</p>
    <button @click="isActive ? pause() : resume()">
      {{ isActive ? 'Pause' : 'Start' }}
    </button>
  </div>
</template>
