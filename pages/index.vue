<template>
  <div>
    <div class="bg-white w-full py-6 mb-2">
      <div class="w-full flex flex-col space-y-2 justify-center">
        <div class="text-4xl font-bold text-center text-gray-800 tracking-wide">{{ this.page.title }}</div>
        <div class="text-xl text-center text-gray-600 tracking-wide">{{ this.subtitle }}</div>
      </div>
    </div>
    <div class="py-6">
      <div class="max-w-3xl mx-auto sm:px-6 lg:max-w-7xl lg:px-8">
        <div
          class="flex flex-row space-x-4 mb-4"
        >
          <div
            v-for="t in Tabs"
            :key="t.name"
            @click="filterCategory(t.name)"
            class="bg-white px-4 py-1 cursor-pointer "
            :class="{ 'border-b-2 bg-yellow-600 text-white font-semibold': filterActive === t.name }"
          >
            <div>{{ t.name }}</div>
          </div>
        </div>

        <main class="main-home">
          <div class="grid grid-cols-12 gap-6">
            <div
              v-for="product in productsAll"
              class="relative px-3 py-4 bg-white shadow col-span-12"
            >
              <div class="w-full grid grid-cols-12 gap-x-2">
                <div class="sm:col-span-2 lg:col-span-2">
                  <div class="aspect-w-1 aspect-h-1 rounded-lg bg-transparent overflow-hidden">
                    <img
                      :src="product.shop.logo[0].url"
                      alt="Back angled view with bag open and handles to the side." class="object-center object-cover"
                    >
                  </div>
                </div>
                <div class="sm:col-span-8 lg:col-span-7 px-4">
                  <div
                    class="uppercase tracking-wide text-lg font-semibold mb-1"
                    :class="{ 'text-blue-900':product.type === 0, 'text-yellow-500':product.type === 1}"
                  > {{ product.type === 0 ? 'Oferta' :  'Codigo'}}</div>
                  <div class="product-title">{{ product.title }}</div>
                  <div class="flex space-x-2 pt-2">
                    <div class="rounded-item" v-if="product.exclusive">Exclusivo</div>
                    <div class="rounded-item" v-if="product.verified">Verificado</div>
                  </div>
                </div>
                <div class="sm:col-span-3 lg:col-span-3">
                  <div class="flex justify-end">
                    <div
                      class="w-40 button-action tracking-wide text-lg font-semibold text-center"
                      :class="{'bg-blue-900':product.type === 0, 'border border-gray-300 border-dashed bg-[#EE5034]':product.type === 1}"
                    >{{ product.type === 0 ? 'Oferta' :  'Ver Cupón'}}</div>
                  </div>
                </div>
              </div>
              <div
                v-if="product.exclusive"
                class="absolute bg-yellow-600 font-normal px-2 py-0.5 uppercase text-white top-0 left-0 mt-3 -ml-2"
              >Exclusivo</div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue, {PropOptions} from 'vue'
import { MetaInfo } from "vue-meta"

interface Page {
  title: string
  description: string
}

interface Product {
  title: string
  description: string
}

const Tabs = [
  {
    name: 'Todos',
    current: true,
  }, {
    name: 'Restaurantes',
    current: false,
  }, {
    name: 'Tecnología',
    current: false,
  }, {
    name: 'Top Cupones',
    current: false,
  },
]

const monthNames = [
  "Enero", "Febrero", "Marzo",
  "Abril", "Mayo", "Junio",
  "Julio", "Agosto", "Septiembre",
  "Octubre", "Noviembre", "Diciembre"
];

export default Vue.extend({
  data() {
    return {
      Tabs,
      filterActive: 'Todos',
      page: {
        type: Object,
      } as PropOptions<Page>,
      products: {
        type: Array,
      } as PropOptions<Product[]>,
    }
  },

  head() :MetaInfo {
    return {
      title: this.page.title as any,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.page.content as string
        }, {
          hid: 'og:title',
          name: 'og:title',
          content: this.page.seo_title as string
        }, {
          hid: 'og:description',
          name: 'og:description',
          content: this.page.seo_desc as string
        }
      ]
    } as {
      title: any
      meta: MetaInfo['meta']
    }
  },
  async asyncData({$axios}): Promise<void | object> {
    const {data} = await $axios.get('/home')
    return {
      page: data.page,
      products: data.data
    }
  },

  computed: {
    subtitle() {
      return this.page.subtitle
        .replace(/:month/g, monthNames[new Date().getMonth()])
        .replace(/:year/g, new Date().getFullYear().toString())
    },
    productsItems() {
      return this.products.tabs
    },
    productsAll(){
      if (!this.products) return []
      return this.filterActive === 'Todos'
        ? Object.keys(this.products.tabs).reduce((acc, tab) => acc.concat(this.products.tabs[tab]), [] as Product[])
        : (Object.prototype.hasOwnProperty.call(this.productsItems, this.filterActive) ? this.productsItems[this.filterActive] : [])
    }
  },

  methods: {
    filterCategory(tab: string) {
      this.filterActive = tab
    }
  }
})
</script>

<style>
.product-title {
  @apply text-2xl font-semibold;
}
.rounded-item {
  @apply bg-gray-200 text-gray-600 px-4 py-0.5 rounded-full;
}
.button-action {
  @apply text-white px-4 py-2;
}

</style>
