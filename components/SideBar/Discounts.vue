<template>
  <div>
    <div class="py-6 text-2xl text-gray-800 font-semibold">Mejores cupones</div>
    <div
      class="best-discounts divide-y divide-gray-200 space-y-4"
    >
      <div
        v-for="d in items"
        class="my-2"
      >
        <div class="flex items-center py-2">
          <div class="flex-shrink-0 content-start">
            <img
              class="h-8 w-8 object-center object-cover border border-gray-200"
              :src="d.shop.logo[0].url"
            />
          </div>
          <div class="ml-4">
            <div class="text-sm leading-5 font-medium text-gray-900 mb-1">
              {{ d.shop.name }}
            </div>
            <div class="text-sm leading-4 text-gray-600 font-light">
              {{ d.description }}
            </div>
          </div>
        </div>
      </div>
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
