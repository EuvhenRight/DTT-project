<script setup lang="ts">
import { RouterLink } from 'vue-router'
import type { headerLinks } from '../types/types'
// STATE VARIABLES for NAVIGATION
const navigationRoutes: headerLinks[] = [
  {
    name: 'Houses',
    title: 'Houses',
    path: '/'
  },
  {
    name: 'About',
    title: 'About',
    path: '/about'
  }
]
</script>

<template>
  <header class="header-nav">
    <div class="container-header">
      <img src="/dtt_logo.png" alt="logo" />

      <nav class="nav-links">
        <RouterLink
          v-for="(routeNav, index) in navigationRoutes"
          :to="routeNav.path"
          :key="index"
          :class="{ active: $route.path === routeNav.path }"
          >{{ routeNav.title }}</RouterLink
        >
      </nav>
    </div>
  </header>
</template>

<style lang="scss">
.header-nav {
  width: 100%;
  display: flex;
  position: fixed;
  justify-content: center;
  background-color: var(--element-background-2);
  font-family: 'Montserrat', sans-serif;
  height: 100px;
  z-index: 999;
  box-shadow: 0px 5px 10px var(--element-tertiary-1);

  & img {
    width: 150px;
  }
}

.container-header {
  width: 1320px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.nav-links a {
  margin-left: 50px;
  font-size: 18px;
  text-decoration: none;
  color: var(--element-tertiary-2);
  font-weight: 500;

  &.active {
    color: var(--text-primary);
    font-weight: 700;
  }
}
@media (min-width: 992px) and (max-width: 1319px) {
  .container-header {
    width: 100%;
    padding: 0 20px;
  }
}

@media screen and (max-width: 991px) {
  .container-header {
    padding: 0 20px;
  }
}
</style>
