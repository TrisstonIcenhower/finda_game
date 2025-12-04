<script setup lang="ts">
import { provide, ref } from 'vue'
import Header from './components/core/Header.vue'
import SearchBar from './components/core/SearchBar.vue'
import GameList from './components/game/GameList.vue'

const DEAL_LINK_ADDR: String = 'https://www.cheapshark.com/redirect?dealID='
/*
  TODO: 
  1. Set up local storage to save users deal list between sessions
  2. Add functionality to "Add to List" buttons on DealCard and GameCard
  3. Set up tiny state management for the deal list maybe using Pinia
*/
provide('dealLinkBase', DEAL_LINK_ADDR)

const searchedGame = ref('')
</script>

<template>
  <Header :class="'glass'"></Header>
  <SearchBar
    :class="'glass'"
    @search="(game) => (searchedGame = game)"
    @clear="searchedGame = ''"
  ></SearchBar>
  <GameList
    :request-type="searchedGame === '' ? 'deals' : 'game'"
    :title="searchedGame === '' ? 'Deals' : 'Search Results for: ' + searchedGame"
  ></GameList>
</template>

<style>
:root {
  --space-indigo: #2b2d42ff;
  --lavender-grey: #7b7ba3ff;
  --sea-green: #09814aff;
  --bright-snow: #f7f7f9ff;
  --bright-snow-glass: hsla(240, 14%, 97%, 0.4);
  --tropical-mint: #2adfb2ff;
  --sandy-brown: #f3a468;
  color: var(--space-indigo);
}

body {
  background-color: var(--space-indigo);
  background-image: radial-gradient(var(--space-indigo), var(--lavender-grey), var(--sea-green));
  animation: backgroundGradient 10s alternate infinite;
  background-size: 500% 500%;
  min-height: 100vh;

  @media (min-width: 800px) {
    margin-left: 20%;
    margin-right: 20%;
  }
}

button,
input {
  border: none;
  color: var(--space-indigo);
  background-color: var(--bright-snow);
}

button {
  border: 1px solid var(--bright-snow);
  border-radius: 8px;
  padding: 4px;
}

button:hover {
  border: 1px solid var(--space-indigo);
}

svg {
  color: var(--bright-snow);
}

a {
  color: var(--lavender-grey);
}

a:hover {
  color: var(--tropical-mint);
}

a:visited {
  color: var(--space-indigo);
}

.glass {
  display: flex;
  justify-content: center;
  align-items: center;
  background: var(--bright-snow-glass);
  border: 1px solid var(--bright-snow);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

.btn-clear {
  background-color: var(--sandy-brown);
}

.btn-submit {
  background-color: var(--tropical-mint);
}

@keyframes backgroundGradient {
  from {
    background-position: 0%;
  }
  to {
    background-position: 100% 100%;
  }
}
</style>
