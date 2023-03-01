<template>
  <Navbar class="navbar" @openDialogue="openDialogue" />

  <div class="text-wrapper">
    <p v-for="line in textLines">{{ line.text }}</p>
  </div>

  <div class="dialogue-container" v-show="showDialogue" @click="showDialogue = false">
    <Dialogue @closeDialogue="showDialogue = false" @click.stop>
      <template #title>Import new greentexts</template>
      <template #content>
        <Importer @stories-added="onStoriesAdded" />
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


//Import dialogue behaviour:
const showDialogue = ref(true)

const openDialogue = function (context) {
  //FUTURE: context may be importNewImage or other, depending
  //on the component that should be mounted in the dialogue. 
  showDialogue.value = true
}


//Stories array management:
const stories = ref([])

const onStoriesAdded = function(newStories) {
  console.log("New story IDs received!", newStories)
}


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
  /* background-color: rgb(0, 0, 0, 0.9); */
  position: absolute;
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
}
</style>


