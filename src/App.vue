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
    margin: 0;
    padding: 0;
    background-color: #313131;
    height: 100vh;
    max-width: 100%;
  }
</style>
