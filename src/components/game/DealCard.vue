<script setup lang="ts">
import { inject, defineProps } from 'vue'
import type { DealInfoResponse } from './GameList.vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faCartArrowDown } from '@fortawesome/free-solid-svg-icons/faCartArrowDown'

const linkBase: String = inject('dealLinkBase', 'example.com')

const props = defineProps<DealInfoResponse>()
</script>

<template>
  <article :class="'glass'" :key="`${props.dealID}`">
    <h2>{{ props.title }}</h2>
    <h3>Is ${{ props.salePrice }} was ${{ props.normalPrice }}</h3>
    <img :src="`${props.thumb}`" :alt="`${props.title} image placeholder`" />
    <h3>
      <FontAwesomeIcon :icon="faCartArrowDown" /><a
        :href="`${linkBase}${props.dealID}`"
        target="_blank"
        >Purchase Link</a
      >
    </h3>
    <button :class="'btn-submit'">Add to List</button>
  </article>
</template>

<style lang="css" scoped>
article {
  display: flex;
  flex-direction: column;
  align-items: start;
}

h2,
h3 {
  margin-left: 16px;
}

img {
  align-self: center;
  width: 50%;
}

button {
  align-self: end;
  margin-right: 16px;
  margin-bottom: 16px;
}

svg {
  color: var(--space-indigo);
}
</style>
