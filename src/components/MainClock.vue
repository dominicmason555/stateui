<script setup lang="ts">
import emitter from 'tiny-emitter/instance'
</script>

<template>
  <div class="timer">
    <span class="clocktext">{{ time }}</span>
    <button @click="startClock">Start</button>
    <button @click="stopClock">Stop</button>
    <button @click="stepClock">Step</button>
  </div>
</template>

<style scoped>
.timer {
  --display: flex;
}
.clocktext {
  font-size: 10em;
}
</style>

<script lang="ts">
export default {
  data() {
    return {
      time: 0,
      running: false
    }
  },
  mounted() {
    setInterval(this.update, 1000)
  },
  methods: {
    startClock() {
      this.running = true
    },
    stopClock() {
      this.running = false
    },
    stepClock() {
      if (this.time >= 9) {
        this.time = 0
        console.log('Clock Overflow')
        emitter.emit('clockOverflow')
      } else {
        this.time += 1
      }
    },
    update() {
      if (this.running) {
        this.stepClock()
      }
    }
  }
}
</script>
