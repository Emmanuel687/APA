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

// FormatDate Start
const formatDate = (dateString) => {
  const options = { year: 'numeric', month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit' };
  return new Date(dateString).toLocaleString(undefined, options);
};
// FormatDate End


// OnMounted Start
onMounted(() => {
  fetchItems()
  fetchCategoryItem()
})
// OnMounted End



</script>

<template>
  <div class="bg-gray-300 min-h-screen p-4">

    <!-- Select Category Section Start -->
    <div class="flex flex-wrap items-center justify-center gap-2 mb-6">
      <button @click="fetchCategoryItem(item)" v-for="(item, index) of itemList" :key="index"
        class="bg-gray-400 hover:bg-gray-500 text-black rounded-xl px-3 py-2 text-sm md:text-base transition duration-300 ease-in-out">
        {{ item }}
      </button>
    </div>
    <!-- Select Category Section End -->

    <!-- Category List Start Card -->
    <div class="mx-auto max-w-[90%]  grid grid-cols-1 md:grid-cols-2 gap-6 mt- 4 ">
      <div v-for="(item, index) in categoryItem.result" :key="item.id"
        class="bg-white shadow-lg rounded-lg overflow-hidden flex flex-col sm:flex-row mx-auto h-[350px]">
        <div class="w-full sm:w-1/3 h-48 sm:h-auto flex-shrink-0">
          <img :src="item.icon_url" alt="Chuck Norris" class="w-full h-full object-cover">
        </div>
        <div class="w-full sm:w-2/3 p-4 sm:p-6 flex flex-col">
          <h2 class="text-xl font-bold mb-2 capitalize text-red-300  p-2 ">
            {{ item.categories[0] || 'Uncategorized' }}
          </h2>

          <div class="flex">

            <p class="mb-1 text-sm bg-gray-100 p-2 rounded"> -> Created: {{ formatDate(item.created_at) }}</p>

          </div>
          <p class="text-sm text-gray-600 mb-1 bg-gray-100 p-2 rounded"> -> ID: {{ item.id }}</p>
          <p class="text-sm text-gray-500 mb-2 bg-gray-100 p-2 rounded">
            -> Updated: {{ formatDate(item.updated_at) }}
          </p>
          <div class="flex-grow overflow-auto bg-gray-100 p-2 rounded flex space-x-4">
            <p class="text-gray-700 text-sm ">{{ item.value }}</p>
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-check" width="44" height="44"
              viewBox="0 0 24 24" stroke-width="1.5" stroke="#ff4500" fill="none" stroke-linecap="round"
              stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path d="M5 12l5 5l10 -10" />
            </svg>
          </div>
        </div>
      </div>
    </div>
    <!-- Category List End Card -->

  </div>
</template>

<style scoped>
.overflow-auto {
  max-height: 150px;
  /* Adjust this value as needed */
  scrollbar-width: thin;
  scrollbar-color: #718096 #EDF2F7;
}

.overflow-auto::-webkit-scrollbar {
  width: 8px;
}

.overflow-auto::-webkit-scrollbar-track {
  background: #EDF2F7;
}

.overflow-auto::-webkit-scrollbar-thumb {
  background-color: #718096;
  border-radius: 20px;
  border: 3px solid #EDF2F7;
}
</style>