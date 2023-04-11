<template>
  <div :class="`app-container ${currentTheme}`">
    <RouterView @changeTheme="changeTheme" />
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref } from 'vue';

const currentTheme = ref("tomorrow")

const changeTheme = function(newTheme) {
  currentTheme.value = newTheme
}
</script>

<style scoped>
</style>
