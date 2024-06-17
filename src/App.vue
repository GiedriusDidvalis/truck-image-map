<script setup>
import { ref } from 'vue'
import Sidebar from './components/Sidebar.vue'
import { db, filterOptions, tableHead} from './db/parts';

const parts = ref(db);
const activeFilter = ref('all');
const handlePartClick = (partSelector) => {
  activeFilter.value = partSelector;
}
</script>

<template>
  <main class="grid">
    <Sidebar @selectChange="handlePartClick" :db="parts" :filterOptions="filterOptions" :tableHead="tableHead" :activeFilter="activeFilter" />

    <div class="image-wrap">
      <h2>Truck Image Map</h2>

      <div style="display: flex; gap: 40px; margin-left: 40px;">

          <svg width="900" height="600" viewBox="0 0 900 600">
              <image href="./assets/truck.png" style="width: 100%; height: 100%;" />

              <!-- Mirror Left -->
              <polygon :class="{'focus': activeFilter === 'mirrors_left'}" @click="handlePartClick('mirrors_left')" class="truck-part" points="131, 142 157, 142 135, 239 130, 239" />

              <!-- Mirror Right -->
              <polygon :class="{'focus': activeFilter === 'mirrors_right'}" @click="handlePartClick('mirrors_right')" class="truck-part" points="490, 121 531, 121 531, 231 490, 231" />

              <!-- Windshield -->
              <polygon :class="{'focus': activeFilter === 'windshield'}" @click="handlePartClick('windshield')" class="truck-part" points="160, 141 457, 131 453, 241 142, 246" />

              <!-- Bumper -->
              <polygon :class="{'focus': activeFilter === 'bumper'}" @click="handlePartClick('bumper')" class="truck-part" points="125, 409 462, 418 462, 547 123, 525" />

              <!-- Headlamp Left -->
              <polygon :class="{'focus': activeFilter === 'headlamps_left'}" @click="handlePartClick('headlamps_left')" class="truck-part" points="127, 431 162, 431 168, 486 129, 478" />

              <!-- Headlamp Right -->
              <polygon :class="{'focus': activeFilter === 'headlamps_right'}" @click="handlePartClick('headlamps_right')" class="truck-part" points="380, 441 434, 443 434, 491 371, 498" />
          </svg>
      </div>
    </div>
  </main>
</template>

<style scoped>
.grid {
  gap: 24px;
  padding-right: 24px;
  display: grid;
  height: 100vh;
  max-height: 100vh;
  width: 100vw;
  grid-template-columns: max-content auto;
}

.image-wrap {
  align-self: baseline;
  justify-self: center;
  text-align: center;
}

.truck-part {
    opacity: 0;
    fill: aqua;

    &:hover,
    &.focus {
        opacity: .3;
    }
}
</style>
