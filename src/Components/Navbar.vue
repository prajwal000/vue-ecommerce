<template>
  <div>
    <Disclosure as="nav" class="bg-gray-800" v-slot="{ open }">
      <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
        <div class="relative flex h-16 items-center justify-between">
          <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
            <!-- Mobile menu button-->
            <DisclosureButton class="relative inline-flex items-center justify-center rounded-md p-2 text-gray-400 hover:bg-gray-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white">
              <span class="absolute -inset-0.5" />
              <span class="sr-only">Open main menu</span>
              Menu
            </DisclosureButton>
          </div>
          <div class="flex flex-1 items-center justify-center sm:items-stretch sm:justify-start">
            <div class="flex flex-shrink-0 items-center">
              <h2 class="font-bold text-white">E-commerce</h2>
            </div>
            <div class="hidden sm:ml-6 sm:block">
              <div class="flex space-x-4">
                <a class="text-white" href="/">Filter</a>
              </div>
            </div>
          </div>
          <div class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
            <!-- Search bar -->
            <input type="text" class="m-3 p-1" placeholder="Search your items here" v-model="searchKeyword" @input="searchItems"/>
          </div>
        </div>
      </div>
    </Disclosure>
    <div class="search-box">
      <div class="searched p-2"  v-for="item in filteredItems" :key="item.id">
        <div v-for="item in filteredItems" :key="item.id" class="searched-box">{{ item.title.slice(0,15) }}</div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import { Disclosure, DisclosureButton } from '@headlessui/vue';

const searchKeyword = ref('');
const products = ref([]);
const filteredItems = ref([]);

const searchItems = async () => {
  
  if (searchKeyword.value.length > 2) {
    try {
      const response = await axios.get("https://fakestoreapi.com/products");
      products.value = response.data;
      filterItems();
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  } else {
    filteredItems.value = []; 
  }
};

const filterItems = () => {
  filteredItems.value = products.value.filter(item => {
    return item.category && item.category.toLowerCase().includes(searchKeyword.value.toLowerCase());
  });
};

watch(searchKeyword, () => {
  searchItems();
});
</script>
