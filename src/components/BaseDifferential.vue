<template>
  <div>
    <h3>{{ baseDifferential }}</h3>
    <div class="buttons">
      <button @click="props.baseDifferential.value++">+</button>
      <button @click="props.baseDifferential.value--">-</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, ref, watch } from 'vue'

const props = defineProps<{ baseDifferential: number }>()

const baseDifferential = watch(
  () => props.baseDifferential,
  (newVal) => {
    baseDifferential.value = newVal
  },
)
</script>

<style scoped></style>
