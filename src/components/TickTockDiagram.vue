<script setup lang="ts">
import emitter from 'tiny-emitter/instance'
</script>

<template>
  <svg width="100mm" height="120mm" version="1.1" viewBox="0 -5 90 115" xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink">
    <defs>
      <marker id="Triangle" overflow="visible" markerHeight="1" markerWidth="1" orient="auto-start-reverse"
        preserveAspectRatio="xMidYMid" viewBox="0 0 1 1">
        <path transform="scale(.5)" d="m6 0-9 5v-10z" fill="context-stroke" fill-rule="evenodd" stroke="context-stroke"
          stroke-width="1pt" />
      </marker>
      <linearGradient id="linearGradient6" x1="70" x2="70" y1="32" y2="92" gradientUnits="userSpaceOnUse">
        <stop stop-color="#c39bff" offset=".2" />
        <stop stop-color="#9bf5ff" offset=".7" />
      </linearGradient>
      <linearGradient id="linearGradient8" x1="128" x2="128" y1="20" y2="81" gradientUnits="userSpaceOnUse"
        spreadMethod="repeat">
        <stop stop-color="#c39bff" offset=".3" />
        <stop stop-color="#9bf5ff" offset=".7" />
      </linearGradient>
    </defs>
    <g transform="translate(-59 -2)">
      <path d="m118 21c46 27 6 56 6 56" fill="none" marker-end="url(#Triangle)" stroke="url(#linearGradient8)"
        stroke-width="2" />
    </g>
    <g transform="translate(-59 -2)">
      <path d="m77 36c-35 24-5 55 5 55" fill="none" marker-start="url(#Triangle)" stroke="url(#linearGradient6)"
        stroke-width="2" />
    </g>
    <g v-if="showTick" id="TickState" transform="translate(-59 -2)">
      <circle id="TickCircle" cx="100" cy="22" r="20" fill="#c39bff" stroke-width="2">
        <animate v-if="currentState == 'Tick'" attributeName="stroke" values="#ffffff;#000000;#ffffff" dur="2s"
          repeatCount="indefinite" />
      </circle>
      <text x="90.031006" y="26.570776" fill="#000000" font-family="sans-serif" font-size="11px" stroke-width=".3"
        style="line-height: 1.25" xml:space="preserve">
        <tspan x="90.031006" y="26.570776" stroke-width=".3">Tick</tspan>
      </text>
    </g>
    <g v-if="showTick" id="TockState" transform="translate(-59 -2)">
      <circle id="TockCircle" cx="100" cy="90" r="20" fill="#9bf5ff" stroke-width="0">
        <animate v-if="currentState == 'Tock'" attributeName="stroke" values="#ffffff;#000000;#ffffff" dur="2s"
          repeatCount="indefinite" />
      </circle>
      <text x="88.839508" y="94.277115" fill="#000000" font-family="sans-serif" font-size="11px" stroke-width=".3"
        style="line-height: 1.25" xml:space="preserve">
        <tspan x="88.839508" y="94.277115" stroke-width=".3">Tock</tspan>
      </text>
      a6
    </g>
  </svg>
  <br />
  <p>
    <span class="stateSpan">Current state: {{ currentState }}</span>
  </p>
  <button @click="toggleState">Toggle State</button>
</template>

<style scoped>
button {
  margin-top: 2em;
}
.stateSpan {
  font-size: 2em;
}
</style>

<script lang="ts">
export default {
  data() {
    return {
      currentState: 'Tick'
    }
  },
  mounted() {
    emitter.on('clockOverflow', this.toggleState)
  },
  methods: {
    setCurrentState(state: String) {
      if (state === 'Tick') {
        this.currentState = 'Tick'
        document.getElementById('TockCircle')?.setAttribute('stroke-width', '0')
        document.getElementById('TickCircle')?.setAttribute('stroke-width', '2')
      } else {
        this.currentState = 'Tock'
        document.getElementById('TickCircle')?.setAttribute('stroke-width', '0')
        document.getElementById('TockCircle')?.setAttribute('stroke-width', '2')
      }
    },
    toggleState() {
      const next = this.currentState == 'Tick' ? 'Tock' : 'Tick'
      this.setCurrentState(next)
    }
  },
  props: {
    showTick: {
      type: Boolean,
      default: true
    },
    showTock: {
      type: Boolean,
      default: true
    }
  }
}
</script>
