<script setup lang="ts">
const duration = ref(60 * 0.05)
const time = ref({
  minutes: 25,
  seconds: 0,
})

const countdown = () => {
  time.value.minutes = Math.floor((duration.value / 60))
  time.value.seconds = Math.floor(duration.value % 60)
  --duration.value
}

const { pause, resume, isActive } = useIntervalFn(() => {
  countdown()
}, 1000, { immediate: false })

watch(duration, (newDuration) => {
  if (newDuration < 0) {
    pause()
    duration.value = 60 * 25
    time.value.minutes = 25
  }
})
</script>

<template>
  <div class="app">
    <span>{{ time.minutes.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}:{{ time.seconds.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}</span>
    <button class="block" @click="isActive ? pause() : resume()">
      {{ isActive ? 'Pause' : 'Start' }}
    </button>
  </div>
</template>

<style scoped lang="scss">
.app {
  min-height: 100vh;
  min-height: 100dvh;
}
</style>
