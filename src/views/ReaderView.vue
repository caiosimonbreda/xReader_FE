<template>
  <Navbar class="navbar" :stories="stories" @openDialogue="openDialogue" @selectStory="selectStory" />

  <div v-if="storyToDisplay.id" class="text-wrapper">
    <h4>{{ storyToDisplay.id }}</h4>
    <p v-for="line in storyToDisplay.text">{{ line.text }}</p>
  </div>

  <div class="dialogue-container" v-show="showDialogue" @click="showDialogue = false">
    <Dialogue @closeDialogue="showDialogue = false" @click.stop>
      <template #title>Import new greentexts</template>
      <template #content>
        <Importer @stories-added="onStoriesAdded" @story-ready="onStoryReady" />
      </template>
    </Dialogue>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Navbar from '../components/TheNavbar.vue'
import Dialogue from '../components/TheDialogue.vue'
import Importer from '../components/TheImport.vue'

// --- Dialogue behaviour: --- 
const showDialogue = ref(true)

const openDialogue = function (context) {
  //FUTURE: context may be importNewImage or other, depending
  //on the component that should be mounted in the dialogue. 
  showDialogue.value = true
}

// --- Stories array management ---
const stories = ref([])
//(sent into the Navbar as a prop)
const onStoriesAdded = function(newStories) {
  console.log("NUTTSACK", newStories)
  stories.value = stories.value.concat(newStories)
}

// --- Story display ---

const storyToDisplay = ref({})

const selectStory = function(index) {
  console.log("New story selected", index);
  storyToDisplay.value = stories.value[index]
  console.log(storyToDisplay.value)
}

const onStoryReady = function(story) {
  const storyIndex = stories.value.map(s => s.id).indexOf(id);
  stories.value[storyIndex].text = story.text;
}

//{ id: xxxx, textLines: [*], }

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
  font-family: Arial, Helvetica, sans-serif;
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


