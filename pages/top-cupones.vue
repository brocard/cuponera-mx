<template>
  <div class="py-6 bg-white">
    <div class="max-w-3xl mx-auto sm:px-6 lg:max-w-7xl lg:px-8 lg:grid lg:grid-cols-12 gap-4">
      <div class="lg:col-span-3 px-3 py-6">
        <LazySideBarShops :items="shops" />
        <LazySideBarDiscounts :items="topDiscounts" />
      </div>
      <main class="lg:col-span-9">
        <div class="flex flex-col py-6">
          <div class="text-3xl">Top Cupones</div>
          <div class="text-xl text-gray-500">{{this.page.subtitle }}</div>
        </div>
        <div class="grid grid-cols-12 gap-6">
          <div
            v-for="item in discounts"
            class="px-3 py-4 bg-[#f2f2f2] shadow col-span-12"
          >
            <div class="w-full grid grid-cols-12 gap-x-2">
              <div class="sm:col-span-2 lg:col-span-2">
                <div class="aspect-w-1 aspect-h-1 rounded-lg bg-transparent overflow-hidden">
                  <img
                    :src="item.shop.logo[0].url"
                    alt="Back angled view with bag open and handles to the side." class="object-center object-cover"
                  >
                </div>
              </div>
              <div class="sm:col-span-8 lg:col-span-7 px-4">
                <div
                  class="uppercase tracking-wide text-lg font-semibold mb-1"
                  :class="{ 'text-blue-900':item.type === 0, 'text-yellow-500':item.type === 1}"
                > {{ item.type === 0 ? 'Oferta' : 'Codigo' }}
                </div>
                <div class="item-title">{{ item.title }}</div>
                <div class="flex space-x-2 pt-2">
                  <div class="rounded-item" v-if="item.exclusive">Exclusivo</div>
                  <div class="rounded-item" v-if="item.verified">Verificado</div>
                </div>
              </div>
              <div class="sm:col-span-3 lg:col-span-3">
                <div class="flex justify-end">
                  <div
                    class="w-40 button-action tracking-wide text-lg font-semibold text-center"
                    :class="{'bg-blue-900':item.type === 0, 'border border-gray-300 border-dashed bg-[#EE5034]':item.type === 1}"
                  >{{ item.type === 0 ? 'Ver Oferta' : 'Ver Cupón' }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script lang="ts">
import Vue, {PropOptions} from 'vue'
import {MetaInfo} from "vue-meta";

interface Page {
  title: string
  description: string
}

interface Discount {
  title: string
  description: string
}

export default Vue.extend({
  data() {
    return {
      page: {
        type: Object,
      } as PropOptions<Page>,
      discounts: {
        type: Array,
      } as PropOptions<Discount[]>,
    }
  },

  head() :MetaInfo {
    return {
      title: this.page.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.page.content as string
        }, {
          hid: 'og:title',
          name: 'og:title',
          content: this.seoTitle as string
        }, {
          hid: 'og:description',
          name: 'og:description',
          content: this.seoDescription as string
        }
      ]
    } as {
      title: any
      meta: MetaInfo['meta']
    }
  },

  async asyncData({$axios}): Promise<void | object> {
    const { data } = await $axios.get('/top-cuponse')
    return {
      page: data.page,
      discounts: data.data.discounts,
      sidebar: data.data.sidebar
    }
  },

  computed: {
    seoTitle() {
      return this.page.title
        .replace(/:month/g, monthNames[new Date().getMonth()])
        .replace(/:year/g, new Date().getFullYear().toString())
    },
    seoDescription() {
      return this.page.seo_desc
        .replace(/:month/g, monthNames[new Date().getMonth()])
        .replace(/:year/g, new Date().getFullYear().toString())
    },
    shops() {
      return this.sidebar.shops
    },
    topDiscounts() {
      return this.sidebar['discounts']
    }
  }
})
</script>

<style scoped>

</style>
