<script setup lang="ts">
import TickTockDiagram from './components/TickTockDiagram.vue'
import MainClock from './components/MainClock.vue'
import BTManager from './components/BTManager.vue'
</script>

<template>
  <main>
    <MainClock></MainClock>
    <div class="diagram">
      <TickTockDiagram></TickTockDiagram>
    </div>
    <BTManager></BTManager>
  </main>
</template>

<style scoped>
.diagram {
  --display: flex;
}
</style>
