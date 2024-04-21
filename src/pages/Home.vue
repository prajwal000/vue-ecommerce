<template>
    <div class="container mx-auto p-3">
      <Menu as="div" class="relative inline-block text-left">
        <div>
          <MenuButton class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
            Filter
          </MenuButton>
        </div>
  
        <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
          <MenuItems class="absolute left-0 z-10 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
            <div class="py-1">
              <MenuItem  v-for="(category, index) in uniqueCategories" :key="index" v-slot="{ active }">
                <span   @click.prevent="handleCategoryClick(category)"  :class="[active ?'bg-gray-100 text-gray-900' : 'text-gray-700', 'block px-4 py-2 text-sm']" class="cursor-pointer">{{ category }}</span>
              </MenuItem>
            </div>
          </MenuItems>
        </transition>
      </Menu>
      <Menu as="div" class="relative inline-block text-left ms-3">
    <div>
      <MenuButton class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
        Sort
      </MenuButton>
    </div>
  
    <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
      <MenuItems class="absolute left-0 z-10 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
        <div class="py-1">
          <MenuItem @click.prevent="sortByPrice('high_to_low')" v-slot="{ active }">
            <span :class="[active ? 'bg-gray-100 text-gray-900' : 'text-gray-700', 'block px-4 py-2 text-sm','cursor-pointer']">High to Low</span>
          </MenuItem>
          <MenuItem @click.prevent="sortByPrice('low_to_high')" v-slot="{ active }">
            <span :class="[active ? 'bg-gray-100 text-gray-900' : 'text-gray-700', 'block px-4 py-2 text-sm','cursor-pointer']">Low to High</span>
          </MenuItem>
        </div>
      </MenuItems>
    </transition>
  </Menu>
  
      <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-6 content pt-3">
        <div v-for="(item, index) in filteredProducts" :key="index" class="bg-white card">
          <div class="img-box">
            <img :src="item.image" :alt="item.title" class="img" />
          </div>
          <div class="p-4">
            <h6 class="text-gray-900 font-semibold text-l mb-2 name">{{ item.title.slice(0,40) }}</h6>
            <div class="text-sm text-slate-600">Price: $ {{ item.price }}</div>
            <div class="text-sm text-slate-600">Rating: <span class="text-green-400">{{ item.rating.rate }}</span></div>
            <div class="text-center py-3">
              <button class="button">View details</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>

  import { Menu, MenuButton, MenuItem, MenuItems } from '@headlessui/vue';
  import { ref, onMounted, computed } from 'vue';
  
  const products = ref([]);
  const filteredProducts = ref([]);
  
  const uniqueCategories = computed(() => {
    const categories = new Set(products.value.map(product => product.category));
    return Array.from(categories);
  });
  

  // category filter
  const handleCategoryClick = (category) => {
    filteredProducts.value = products.value.filter(product => product.category === category);
  };
  const sortByPrice = (order) => {
    if (order === 'high_to_low') {
      filteredProducts.value.sort((a, b) => b.price - a.price);
    } else if (order === 'low_to_high') {
      filteredProducts.value.sort((a, b) => a.price - b.price);
    }
  };
  
  onMounted(() => {
    fetch("https://fakestoreapi.com/products")
      .then(response => response.json())
      .then(data => {
        
        products.value = data; 
        filteredProducts.value = data;
      })
      .catch(error => {
        console.error("Error fetching data:", error);
      });
  });
  </script>
  