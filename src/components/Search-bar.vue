<script setup lang="ts">
import { useHousesStore } from '../stores/store'

import IconClean from '../assets/icons/ic_clear@3x.png'

const housesStore = useHousesStore()
// DELETE VALUE FROM INPUT
const inputClear = () => {
  housesStore.searchValue = null
  housesStore.filteredHouses
}
// SEARCH VALUE FROM INPUT
const inputSearch = () => {
  housesStore.filteredHouses
}
</script>

<template>
  <form class="search-bar-form" autocomplete="off" @submit.prevent="inputSearch">
    <div class="wrapper-search-bar">
      <input
        v-model.trim="housesStore.searchValue"
        name="searchBar"
        type="text"
        placeholder="Search for a house"
        @keyup="inputSearch"
      />
      <img v-if="housesStore.searchValue" @click="inputClear" class="icon-clean" :src="IconClean" />
    </div>
  </form>
</template>

<style scoped>
.search-bar-form {
  display: flex;
  align-items: center;
}
.wrapper-search-bar {
  position: relative;
  display: flex;
  align-items: center;
}
.icon-clean {
  position: absolute;
  right: 15px;
  width: 20px;
  opacity: 0.5;
}
.search-bar-form input {
  width: 450px;
  padding: 10px 50px 10px 50px;
  background: var(--element-tertiary-1) url('../assets/icons/ic_search@3x.png') no-repeat 20px
    center;
  background-size: 15px 15px;
  font-size: 14px;
  border: none;
  border-radius: 5px;
  color: var(--text-secondary);
  font-family: 'Open Sans', sans-serif;
  outline: none;
  font-weight: 400;
  display: flex;
}
.icon-clean:hover {
  cursor: pointer;
  opacity: 1;
  transition: 0.3s ease-in-out;
  transform: scale(1.1);
}
@media (max-width: 806px) {
  .wrapper-search-bar {
    width: 100%;
  }
  .search-bar-form input {
    width: 100%;
  }
}
</style>
