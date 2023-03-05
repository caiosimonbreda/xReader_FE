<template>
  <Navbar class="navbar" :stories="stories" @openDialogue="openDialogue" @selectStory="selectStory" />

  <div class="main-content-wrapper">
    <div v-if="storyToDisplay.id" class="text-wrapper">
      <p v-for="line in storyToDisplay.text"
        :class="line.text[0] === '>' || line.text[0] == line.text[0].toLowerCase() ? 'text-line-green' : 'text-line-regular'">
        {{ line.text }}</p>
    </div>
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
const onStoriesAdded = function (newStories) {
  console.log("NUTTSACK", newStories)
  stories.value = stories.value.concat(newStories)
}

// --- Story display ---

const storyToDisplay = ref({})

const selectStory = function (index) {
  console.log("New story selected", index);
  storyToDisplay.value = stories.value[index]
  console.log(storyToDisplay.value)
}

const onStoryReady = function (story) {
  const storyIndex = stories.value.map(s => s.id).indexOf(story.id);
  stories.value[storyIndex].text = story.text;
}

//{ id: xxxx, textLines: [*], }

</script>

<style scoped>
.navbar {
  position: fixed;
  z-index: 99;
  width: 100vw;
  bottom: 0;
  max-width: 100%;
}

.main-content-wrapper {
  position: absolute;
  height: 100%;
  width: 100%;
  z-index: 1;
}

.text-wrapper {
  background-color: #313131;
  position: relative;
  margin: auto;
  padding-top: 2em;
  padding-bottom: 100px;
  width: 60%;
  font-family: Arial, Helvetica, sans-serif;
}

.text-line-regular {
  color: #6a6a6a;
  line-height: 200%;
}

.text-line-green {
  color: #94bc88;
  line-height: 140%;
}

.dialogue-container {
  display: flex;
  /* background-color: rgb(0, 0, 0, 0.9); */
  position: absolute;
  z-index: 100;
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
}
</style>


