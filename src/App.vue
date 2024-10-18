<script setup>
// Imports Start
import { watch } from 'vue';
import { onMounted, ref } from 'vue';
// Imports End

// Reactive Variables Start
const itemList = ref([])
const category = ref('Food')
const categoryItem = ref('')
const selectedCategory = ref('');

// Reactice Variables End

// Fetch Items Start
const fetchItems = async () => {
  try {
    const response = await fetch('https://api.chucknorris.io/jokes/categories');
    const data = await response.json();
    itemList.value = data
  } catch (error) {
    console.error(error)
  }
}
// Fetch Items Start

// Fetch Items Start
const fetchCategoryItem = async (category = "food") => {
  selectedCategory.value = category;
  try {
    const response = await fetch(`https://api.chucknorris.io/jokes/search?query=${category}`);
    const data = await response.json();
    categoryItem.value = data;
  } catch (error) {
    console.error(error);
  }
};

// Fetch Items Start

const formatDate = (dateString) => {
  const options = { year: 'numeric', month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit' };
  return new Date(dateString).toLocaleString(undefined, options);
};

// OnMounted Start
onMounted(() => {
  fetchItems()
  fetchCategoryItem()
})
// OnMounted End

// Watchers 
watch(category, (oldVal, newVal) => {
  fetchCategoryItem()
})
// Watchers

</script>

<template>
  <div class="bg-gray-300 min-h-screen p-4">

    <!-- Select Category Section Start -->
    <div class="flex flex-wrap items-center justify-center gap-2 mb-6">
      <button @click="fetchCategoryItem(item)" v-for="(item, index) of itemList" :key="index"
        class="bg-gray-400 hover:bg-gray-500 text-white rounded-xl px-3 py-2 text-sm md:text-base transition duration-300 ease-in-out">
        {{ item }}
      </button>
    </div>
    <!-- Select Category Section End -->

    <!-- Category List Start Card -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-4">
      <div v-for="(item, index) in categoryItem.result" :key="item.id"
        class="bg-white shadow-lg rounded-lg overflow-hidden flex flex-col sm:flex-row max-w-2xl mx-auto">
        <div class="w-full sm:w-1/3 h-48 sm:h-auto flex-shrink-0">
          <img :src="item.icon_url" alt="Chuck Norris" class="w-full h-full object-cover">
        </div>
        <div class="w-full sm:w-2/3 p-4 sm:p-6">
          <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-4">
            <h2 class="text-xl font-bold mb-2 sm:mb-0 capitalize text-red-300">{{ item.categories[0] }}</h2>
          </div>
          <p class="mb-1 sm:mb-0">Created: {{ formatDate(item.created_at) }}</p>

          <p class="text-sm text-gray-600 mb-2">ID: {{ item.id }}</p>
          <div class="flex flex-col sm:flex-row justify-between text-xs text-gray-500">
            <p>Updated: {{ formatDate(item.updated_at) }}</p>
          </div>
          <p class="text-gray-700 mb-4">{{ item.value }}</p>

        </div>
      </div>
    </div>
    <!-- Category List End Card -->

  </div>
</template>

<style scoped></style>
