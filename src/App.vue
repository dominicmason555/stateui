<script setup lang="ts">
import TickTockDiagram from './components/TickTockDiagram.vue'
import MainClock from './components/MainClock.vue'
</script>

<template>
  <main>
    <MainClock></MainClock>
    <div class="diagram">
      <TickTockDiagram></TickTockDiagram>
    </div>
  </main>
</template>

<style scoped>
.diagram {
  --display: flex;
}
</style>
