<template>
  <div class="test">
    <ProgramCounter />
    <button @click="step">Step</button>
  </div>
</template>

<script>
import { defineComponent, provide, ref } from 'vue'

import ProgramCounter from './components/ProgramCounter.vue'

export default defineComponent({
  components: {
    ProgramCounter,
  },
  setup() {
    const emitter = {
      events: { },
      on(event, handler) {
        this.events[event] = this.events[event] || []
        this.events[event].push(handler)
      },
      off(event, handler) {
        if (this.events[event]) {
          this.events[event] = this.events[event].filter(h => h !== handler)
        }
      },
      emit(event, ...data) {
        (this.events[event] || []).forEach(handler => handler(...data))
      }
    }
    provide('clock', emitter)

    const counter = ref(0)
    const step = () => emitter.emit('tick', counter.value++)

    return { step }
  },
})
</script>
