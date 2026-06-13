<template>
  <ion-page>

    <ion-content class="bg-gray-300">

      <!-- BUTTON -->
      <div class="flex justify-center mt-5">

        <button
          @click="getCrypto"
          class="bg-blue-600 text-white px-6 py-2 rounded-md shadow-md"
        >
          Refresh
        </button>

      </div>

      <!-- LAST UPDATE -->
      <p class="text-center mt-2 text-sm text-gray-700">
        Last Update: {{ lastUpdate }}
      </p>

      <!-- LIST -->
      <div class="p-4">

        <div
          v-for="crypto in cryptos"
          :key="crypto.id"
          class="bg-yellow-100 border border-yellow-300 p-3 mb-2"
        >

          <div class="flex justify-between items-center">

            <!-- RANK -->
            <div>

              <p class="text-xs">
                Rank
              </p>

              <h1 class="text-3xl font-light">
                {{ crypto.rank }}
              </h1>

            </div>

            <!-- NAME + SYMBOL -->
            <div class="flex-1 ml-4">

              <p class="text-xs">
                {{ crypto.name }}
              </p>

              <h2 class="text-3xl font-bold">
                {{ crypto.symbol }}
              </h2>

            </div>

            <!-- PRICE -->
            <div>

              <p class="text-xs">
                USD
              </p>

              <h2 class="text-2xl">
                {{ crypto.price_usd }}
              </h2>

            </div>

          </div>

        </div>

      </div>

    </ion-content>

  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonPage
} from '@ionic/vue'

import { ref, onMounted } from 'vue'
import axios from 'axios'

interface Crypto {
  id: string
  rank: number
  name: string
  symbol: string
  price_usd: string
}

const cryptos = ref<Crypto[]>([])

const lastUpdate = ref('')

const getCrypto = async () => {

  try {

    const response = await axios.get(
      'https://api.coinlore.net/api/tickers/'
    )

    cryptos.value = response.data.data.slice(0, 7)

    lastUpdate.value = new Date().toLocaleTimeString()

  } catch (error) {

    console.log(error)

  }

}

onMounted(() => {

  getCrypto()

})
</script>