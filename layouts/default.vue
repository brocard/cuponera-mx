<template>
  <div class="min-h-screen bg-gray-[#F5F5F5]">
    <header class="bg-white shadow-sm lg:static lg:overflow-y-visible">
      <PartialsTopBar />
    </header>
    <div class="bg-gray-100">
      <Nuxt />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  mounted () {}
})
</script>

<style scoped>

</style>
