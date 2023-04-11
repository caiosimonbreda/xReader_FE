<template>
  <div class="reader-view">
    <Navbar 
      class="navbar-container" 
      :stories="stories" 
      :selected-story-index="selectedStoryIndex" 
      @openDialogue="openDialogue" 
      @selectStory="selectStory" 
      @deleteStory="deleteStory"
      @changeTheme="selectTheme"
    />
    <div class="main-content-wrapper">
      <div v-if="storyToDisplay.text" class="text-wrapper">
        <p 
          v-for="line in storyToDisplay.text"
          :class="line.class"
        >
          {{ line.text }}</p>
      </div>
      <div v-if="storyToDisplay.id && !storyToDisplay.text" class="loader-wrapper">
        <div class="lds-dual-ring"></div>
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
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Navbar from '../components/TheNavbar.vue'
import Dialogue from '../components/TheDialogue.vue'
import Importer from '../components/TheImport.vue'

// --- Emits: ---

const emit = defineEmits(['changeTheme'])

const selectTheme = function(theme) {
  emit("changeTheme", theme)
}

// --- Dialogue behaviour: --- 
const showDialogue = ref(true)

const openDialogue = function (context) {
  //FUTURE: context may be importNewImage or other, depending
  //on the component that should be mounted in the dialogue. 
  showDialogue.value = true
}

// --- Stories array management: ---
const stories = ref([])
//(sent into the Navbar as a prop)

// adding new stories:
const onStoriesAdded = function (newStories) {
  console.log(newStories)
  stories.value = stories.value.concat(newStories)
  selectStory(stories.value.length - 1)
}
// deleting a story:
const deleteStory = function (index) {
  stories.value.splice(index, 1)

  if (stories.value[index-1]) {
    selectStory(index-1)
  } else if (stories.value[index+1]) {
    selectStory(index+1)
  } else {
    storyToDisplay.value = {}
    selectedStoryIndex.value = null
  }
}


// --- Story display: ---
const storyToDisplay = ref({})
const selectedStoryIndex = ref(null)

const selectStory = function (index) {
  selectedStoryIndex.value = index
  storyToDisplay.value = stories.value[index]
}

const onStoryReady = function (story) {
  const storyIndex = stories.value.map(s => s.id).indexOf(story.id);
  story.text.forEach((line, index) => {

    if (line.text[0] == '>' && line.text[1] != '>') {
      line.class = 'green-text'
    } else if (line.text[0].toLowerCase() == line.text[0] && story.text[index-1]) {
      line.class = story.text[index-1].text[0] == '>' ? 'green-text' : 'normal-text'
    } else {
      line.class = 'normal-text'
    }

  })
  stories.value[storyIndex].text = story.text;
}
</script>

<style scoped>

</style>


