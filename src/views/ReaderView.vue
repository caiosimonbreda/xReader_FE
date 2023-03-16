<template>
  <Navbar class="navbar" :stories="stories" @openDialogue="openDialogue" @selectStory="selectStory" />
<!-- :class="line.text[0] === '>' || line.text[0] == line.text[0].toLowerCase() ? 'text-line-green' : 'text-line-regular'"> -->
  <div class="main-content-wrapper">
    <div v-if="storyToDisplay.id" class="text-wrapper">
      <p 
        v-for="line in storyToDisplay.text"
        :style="`color: ${line.color};`"
      >
        {{ line.text }}</p>
    </div>
  </div>

  <div class="dialogue-container" v-show="showDialogue" @click="showDialogue = false">
    <Dialogue @closeDialogue="showDialogue = false" @click.stop>
      <template #title>Import new greentexts</template>
      <template #content>
        <Importer @stories-added="onStoriesAdded" @story-ready="onStoryReady" @closeDialogue="showDialogue = false" />
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
  selectStory(stories.value.length - 1)
}

// --- Story display ---
const storyToDisplay = ref({})

const selectStory = function (index) {
  console.log("New story selected", stories.value[index]);
  storyToDisplay.value = stories.value[index]
}

const onStoryReady = function (story) {
  const storyIndex = stories.value.map(s => s.id).indexOf(story.id);
  console.log(`storyIndex is ${storyIndex}`)
  story.text.forEach((line, index) => {

    if (line.text[0] == '>' && line.text[1] != '>') {
      line.color = '#869F5F'
    } else if (line.text[0].toLowerCase() == line.text[0] && story.text[index-1]) {
      line.color = story.text[index-1].text[0] == '>' ? '#869F5F' : '#888'
    } else {
      line.color = '#888'
    }

  })
  stories.value[storyIndex].text = story.text;
}
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


