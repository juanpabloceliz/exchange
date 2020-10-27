<template>
  <div class="flex-col">
    <div class="flex justify-center">
      <bounce-loader :loading="isLoading" :color="'#68d391'" :size="100" />
    </div>

    <template v-if="!isLoading">
      <div class="flex flex-col sm:flex-row justify-around items-center">
        <div class="flex flex-col items-center">
          <img class="w-20 h-20 mr-5" 
            :src="`https://static.coincap.io/assets/icons/${asset.symbol.toLowerCase()}@2x.png`"
            :alt="asset.name">
          <h1 class="text-5xl">
            <small class="sm:mr-2 text-gray-500">{{ asset.symbol }}</small>
          </h1>
        </div>

        <div class="my-10 flex flex-col">
          <ul>
            <li class="flex justify-between">
                <b class="text-gray-600 mr-10 uppercase">Ranking</b>
                <span>#{{ asset.rank | dollar }}</span>
            </li>
            <li class="flex justify-between">
                <b class="text-gray-600 mr-10 uppercase">Precio actual</b>
                <span>{{asset.priceUsd | dollar }}</span>
            </li>
            <li class="flex justify-between">
                <b class="text-gray-600 mr-10 uppercase">Precio más bajo</b>
                <span>{{ min | dollar }} </span>
            </li>
            <li class="flex justify-between">
                <b class="text-gray-600 mr-10 uppercase">Precio promedio</b>
                <span>{{ max | dollar }} </span>
            </li>
            <li class="flex justify-between">
                <b class="text-gray-600 mr-10 uppercase">Variación 24hs</b>
                <span>{{ asset.changePercente24Hr | percent }}</span>
            </li>
          </ul>
        </div>

        <div class="my-10 sm:mt-0 flex flex-col justify-center text-center">
          <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
            Cambiar
          </button>

          <div class="flex flex-row my-5">
            <label class="w-full" for="converValue">
              <input type="number" id="convertValue" class="text-center bg-white focus:outline-none focus:shadow-outline border">
            </label>
          </div>

          <span class="text-xl"></span>
        </div>
      </div>

      <line-chart 
        class="my-10"
        :colors="['orange']"
        :min="min"
        :max="max"
        :data="history.map(h => [h.date, parseFloat(h.priceUsd).toFixed(2)])"
      />

      <h3 class="text-xl my-10">Mejores Ofertas de Cambio</h3>
      <table>
        <tr v-for="m in markets" :key="`${m.exchangeId}-${m.priceUsd}`" class="border-b">
          <td>
            <b>{{ m.exchangeId }}</b>
          </td>
          <td>{{ m.priceUsd | dollar}}</td>
          <td>{{ m.baseSybol }} / {{ m.quoteSymbol }}</td>
          <td>
            <px-button
              :is-loading="m.isLoading || false"
              v-if="!m.url"
              @click="getWebsite(m)">
              <slot>Obtener Link</slot>
            </px-button>
            <a v-else class="hover:underline text-green-600" target="_blanck">
              {{ m.url }}
            </a>
          </td>
        </tr>
      </table>
    </template>
  </div>
</template>

<script>
import PxButton from '@/components/PxButton'
import api from '@/api'

export default {
  name: 'CoinDetail',

  data() {
    return {
      isLoading: false,
      asset: {},
      history: [],
      markets: []
    }
  },

  components: { PxButton },

  computed: {
    min () {
      return Math.min(
        ... this.history.map(h=> parseFloat(h.priceUsd).toFixed(2))
      )
    },
    max () {
      return Math.max(
        ... this.history.map(h=> parseFloat(h.priceUsd).toFixed(2))
      )
    },
    avg() {
      return this.history.reduce((a, b) => a + parseFloat(b.priceUsd), 0) / this.history.length
    }
  },

  created () {
    this.getCoin()
  },

  methods: {
    getWebsite (exchange) {
      this.$set(exchange, 'isLoading', true)

      return api.getExchange(exchange.exchangeId)
        .then(res => {
          this.$set(exchange, 'url', res.exchangeUrl)
        })
        .finally(()=> this.$set(exchange, 'isLoading', false) )
    },

    getCoin () {
      const id = this.$route.params.id
      this.isLoading = true

      Promise.all([
        api.getAsset(id),
        api.getAssetHistory(id),
        api.getMarket(id)
      ])
        .then(([asset, history, markets]) => {
          this.asset = asset
          this.history = history
          this.markets = markets
        })
        .finally(() => this.isLoading = false)
    }
  }
}
</script>