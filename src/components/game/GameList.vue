<script setup lang="ts">
import { defineProps, onMounted, onUpdated, ref } from 'vue'
import DealCard from './DealCard.vue'
import axios from 'axios'
import GameCard from './GameCard.vue'

const DEAL_API_ADDR: String =
  'API KEY HERE, POSTMAN temporary proxy used for development purposes only'

export interface DealInfoResponse {
  dealID: String
  title: String
  storeID: String
  salePrice: String
  normalPrice: String
  savings: String
  metacriticScore: String
  steamRatingText: String
  releaseDate: String
  dealRating: String
  thumb: String
}

export interface GameInfoResponse {
  gameID: String
  steamAppID: String
  cheapest: String
  cheapestDealID: String
  external: String
  internalName: String
  thumb: String
}

const games = ref<GameInfoResponse[]>([])
const deals = ref<DealInfoResponse[]>([])

const props = defineProps({
  requestType: {
    type: String,
    default: 'deals',
  },
  title: String,
  searchedGame: {
    type: String,
    default: '',
  },
})

const request = ref(props.requestType)

async function fetchDeals() {
  await axios.get<DealInfoResponse[]>(`${DEAL_API_ADDR}deals`).then((response) => {
    deals.value = response.data
    console.log(deals.value)
  })
}

async function fetchGame() {
  console.log(props.searchedGame + ' in fetchGame')
  await axios.get(`${DEAL_API_ADDR}games?title=${props.searchedGame}`).then((response) => {
    games.value = response.data
    console.log(games.value)
  })
}

onMounted(() => {
  fetchData()
})

onUpdated(() => {
  if (request.value !== props.requestType) {
    request.value = props.requestType
    fetchData()
  }
})

function fetchData() {
  if (request.value === 'deals') {
    fetchDeals()
  } else if (request.value === 'game') {
    fetchGame()
  }
}
</script>

<template>
  <h1>{{ title }}</h1>
  <section>
    <ul>
      <DealCard
        v-if="props.requestType === 'deals'"
        v-for="deal in deals"
        :key="deal.dealID"
        v-bind="deal"
      ></DealCard>
      <GameCard v-else v-for="game in games" :key="game.gameID" v-bind="game"></GameCard>
    </ul>
  </section>
</template>

<style lang="css" scoped>
ul {
  list-style-type: none;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 16px;
}
</style>
