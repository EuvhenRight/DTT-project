<script setup lang="ts">
import { useMonitorSize } from '../utils/monitor-sizes'

import IconPlusMobile from '../assets/icons/ic_plus_grey@3x.png'
import IconPlus from '../assets/icons/ic_plus_white@3x.png'

const { base, xs, sm } = useMonitorSize()
</script>

<template>
  <div class="container-create-btn">
    <button v-if="!base && !xs && !sm" class="create-btn">
      <span>
        <img :src="IconPlus" alt="IconPlus" />
      </span>
      <span class="create-btn-text">CREATE NEW</span>
    </button>
    <!-- MOBILE VIEW -->
    <button v-if="base || xs || sm" class="create-btn-mobile">
      <span>
        <img :src="IconPlusMobile" alt="IconPlusMobile" />
      </span>
    </button>
  </div>
</template>

<style scoped>
.container-create-btn:hover {
  & img {
    transition: 0.5s ease-in-out;
    transform: rotate(90deg);
  }
}

.container-create-btn {
  display: flex;
  justify-content: center;
  align-items: center;
}
.create-btn {
  padding: 10px;
  background-color: var(--element-primary);
  border: none;
  border-radius: 8px;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  & img {
    height: 20px;
    margin-right: 20px;
  }
}
.create-btn-mobile {
  padding: 0;
  background-color: var(--element-background-1);
  width: auto;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;

  & img {
    height: 20px;
    margin-right: 20px;
  }
}
.create-btn-text {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 18px;
  color: var(--element-background-2);
}

@media (max-width: 1024px) {
  .create-btn-mobile {
    & img {
      margin: 0;
    }
  }
}
</style>
