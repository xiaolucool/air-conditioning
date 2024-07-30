<script setup lang="ts">
import Index from '@/pages/index.vue'
import { ref } from 'vue'
let selectedOption = ref(0)
const indexRef = ref()
const handleSelection = () => {

  indexRef.value.onColor()
  window.localStorage.setItem('selectedOption', JSON.stringify(selectedOption.value))
}
const onLoadOption = () => {
  let num = Number(window.localStorage.getItem('selectedOption'))
  if (num) {
    selectedOption.value = num
  } else {
    selectedOption.value = 0
  }
}
onLoadOption()


</script>

<template>
  <header>
    <h2>{{ selectedOption == 0 ? '布布' : selectedOption == 1 ? '一二' : '布布' }}空调</h2>
    <select  v-model="selectedOption" @change="handleSelection">
      <option :value="0">布布</option>
      <option :value="1">一二</option>
    </select>
  </header>

  <main>
    <Index ref="indexRef" :selectedOption="selectedOption" />
  </main>
</template>

<style scoped>
header {
  text-align: center;
}
</style>
