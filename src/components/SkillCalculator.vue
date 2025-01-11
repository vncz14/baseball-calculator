<template>
  <main>
    <div class="options">
      <div class="skill-level">
        <label for="skill-level">Current skill level</label>
        <input type="text" v-model="skillLevel" />
      </div>
      <div class="base-differential">
        <label for="base-differential">Base differential</label>
        <h3>{{ baseDifferential }}</h3>
        <div class="buttons">
          <button @click="baseDifferential++">+</button>
          <button @click="baseDifferential--">-</button>
        </div>
      </div>
    </div>
    <div class="result">
      <p>New skill level</p>
      <h1>[{{ truncate(lowerBound, 1) }}, {{ truncate(upperBound, 1) }}]</h1>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const baseDifferential = ref(0)
const skillLevel = ref('0')

const lowerBound = ref(0)
const upperBound = ref(0)

function truncate(num: number, decPlaces: number) {
  return Math.trunc(num * Math.pow(10, decPlaces)) / Math.pow(10, decPlaces)
}

function setBounds(skillLevel: number, baseDifferential: number) {
  const baseDifferentialBonus = 97.5 * Math.atan(baseDifferential / 10)

  const skillWithoutCpuBonus = 0.91 * skillLevel + baseDifferentialBonus

  let lowCpuBonus = (skillLevel - 50) / 8
  lowCpuBonus = lowCpuBonus < 0 ? 0 : lowCpuBonus

  let highCpuBonus = (skillLevel + 50) / 8
  highCpuBonus = highCpuBonus > 150 ? 150 : highCpuBonus

  let skillWithLowCpuBonus = skillWithoutCpuBonus + lowCpuBonus
  skillWithLowCpuBonus = skillWithLowCpuBonus < 0 ? 0 : skillWithLowCpuBonus
  skillWithLowCpuBonus = skillWithLowCpuBonus > 10000 ? 10000 : skillWithLowCpuBonus

  let skillWithHighCpuBonus = skillWithoutCpuBonus + highCpuBonus
  skillWithHighCpuBonus = skillWithHighCpuBonus < 0 ? 0 : skillWithHighCpuBonus
  skillWithHighCpuBonus = skillWithHighCpuBonus > 10000 ? 10000 : skillWithHighCpuBonus

  lowerBound.value = skillWithLowCpuBonus
  upperBound.value = skillWithHighCpuBonus
}

watch(skillLevel, () => {
  baseDifferential.value = 0
  setBounds(parseInt(skillLevel.value), baseDifferential.value)
})

watch(baseDifferential, () => {
  setBounds(parseInt(skillLevel.value), baseDifferential.value)
})
</script>

<style scoped>
main {
  display: flex;
  gap: 2rem;
  flex-direction: column;
  padding: 2rem;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.options > div {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.skill-level > input {
  width: 5rem;
  font-size: 1.5rem;
}

.buttons {
  display: flex;
  gap: 1rem;
}

.buttons > button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border: 1px solid black;
  border-radius: 0.25rem;
  background-color: white;
  cursor: pointer;
}
</style>
