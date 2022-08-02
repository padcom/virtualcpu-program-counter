<template>
  <div class="clock">
    <h2>Program counter</h2>
    <fieldset>
      <legend>Current state</legend>
      <div>Value: {{ counter }}</div>
    </fieldset>
    <div class="actions">
      <button @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
import { ref, defineComponent, inject, onMounted, onBeforeUnmount } from 'vue'

export default defineComponent({
  setup() {
    const clock = inject('clock')
    const counter = ref(0)
    const reset = () => counter.value = 0
    const tick = () => { counter.value = ++counter.value & 0xffff }
    onMounted(() => clock.on('tick', tick))
    onBeforeUnmount(() => clock.on('tick', tick))

    return { counter, reset }
  },
})
</script>
