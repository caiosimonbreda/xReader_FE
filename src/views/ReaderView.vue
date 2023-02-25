<template>
  <Navbar class="navbar" />

  <div class="text-wrapper">
    <p v-for="line in textLines">{{ line.text }}</p>
  </div>

  <div class="dialogue-container">
    <Dialogue>
      <template #title>Lmao even</template>
      <template #content>
        <Importer />
      </template>
    </Dialogue>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Navbar from '../components/TheNavbar.vue'
import Dialogue from '../components/TheDialogue.vue'
import Importer from '../components/TheImport.vue'

const textLines = ref("")

</script>

<style scoped>
.navbar {
  position: absolute;
  width: 100vw;
  bottom: 0;
  max-width: 100%;
}
.text-wrapper {
  margin: auto;
  width: 90%;
  font-family: 'Source Code Pro', 'Courier New', Courier, monospace;
  color: #94bc88
}
.dialogue-container {
  display: flex;
  background-color: transparent;
  position: absolute;
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
}
</style>


