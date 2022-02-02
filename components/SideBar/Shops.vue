<template>
  <div
    class="shops-items grid grid-cols-3 gap-4"
  >
    <div
      v-for="s in items"
      class="border border-gray-200 h-20 aspect-w-1 aspect-h-1"
    >
      <img class="w-full h-full object-center object-cover" :src="s.logo[0].url">
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    items: {
      type: Array,
      required: true
    }
  },
})
</script>

<style scoped>

</style>
