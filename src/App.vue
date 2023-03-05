<template>
  <div class="app-container">
    <RouterView />
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<style scoped>
  .app-container {
    position: relative;
    margin: 0;
    padding: 0;
    background-color: #313131;
    min-height: 100vh;
    max-height: fit-content;
    max-width: 100%;
  }
</style>
