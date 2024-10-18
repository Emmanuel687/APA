<script setup>
// Imports Start
import { onMounted, ref } from 'vue';
// Imports End

// Reactive Variables Start
const itemList = ref([])
// Reactice Variables End

// Fetch Items Start
const fetchItems = async () => {
  try {
    const response = fetch('https://api.chucknorris.io/jokes/categories');
    const data = await response.json();
    itemList.value = data
  } catch (error) {
    console.error(error)
  }
}
// Fetch Items Start

// OnMounted Start
onMounted(() => {
  fetchItems()
})
// OnMounted End


</script>

<template>
  <div>
    ghvjbnk
    <!-- {{ itemList }} -->


  </div>
</template>

<style scoped></style>
