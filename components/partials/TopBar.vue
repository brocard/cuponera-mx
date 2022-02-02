<template>
  <nav class="bg-gray-100">
    <div class="max-w-7xl mx-auto px-2 sm:px-4 lg:px-8">
      <div class="flex justify-between h-16">
        <div class="flex px-2 lg:px-0">
          <div class="flex-shrink-0 flex items-center">
            <h1 class="text-2xl text-[#515151]">Logo</h1>
          </div>
          <div class="hidden lg:ml-6 lg:flex lg:space-x-8">
            <NuxtLink
              to="/"
              class="inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            >Home</NuxtLink>
            <NuxtLink
              to="/top-cupones"
              class="border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            >Top 20</NuxtLink>
          </div>
        </div>
        <div class="flex-1 flex items-center justify-center px-2 lg:ml-6 lg:justify-end">
          <div class="max-w-lg w-full lg:max-w-xs">
            <label for="search" class="sr-only">Search</label>
            <div class="relative">
              <input
                id="search" name="search"
                type="search"
                class="block w-full pl-5 pr-3 py-2 border border-gray-50 rounded-xs leading-5 bg-white placeholder-gray-500 focus:outline-none focus:placeholder-gray-400 focus:ring-1 focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                autocomplete="off"
                placeholder="Buscar"
              />
              <div class="absolute inset-y-0 right-0 px-3 flex items-center pointer-events-none">
                <IconsSearch />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({})
</script>

<style scoped>
  .nuxt-link-active {
    @apply border-transparent;
  }
  .nuxt-link-exact-active {
    @apply border-yellow-600 text-gray-900;
  }
</style>
