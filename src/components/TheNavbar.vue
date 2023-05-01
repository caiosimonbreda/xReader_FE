<template>
  <nav class="navbar">
    <div class="navbar-story-list-wrapper">
      <div class="navbar-puller" @click="toggleNavbar">
        <span class="material-symbols-outlined navbar-icon">expand_less</span>
      </div>
      <div v-for="story, index in props.stories" :class="`navbar-story ${getButtonClass(index)}`">
        <button @click.stop="onStoryClick(index)" class="navbar-story-button">
          {{ story.fileName }}
        </button>
        <button class="navbar-story-delete-button" @click.stop="onDeleteStory(index)">
          <span class="material-symbols-outlined navbar-story-delete-button-icon">close</span>
        </button>
      </div>
    </div>

    <div class="navbar-controls">
      <button class="navbar-input" id="add-stories-btn" @click="openImporter"><span
          class="material-symbols-outlined navbar-icon">add</span></button>
      <div class="font-size-controls">
        <button class="navbar-input"><span class="material-symbols-outlined navbar-icon">text_decrease</span></button>
        <button class="navbar-input"><span class="material-symbols-outlined navbar-icon">text_increase</span></button>
      </div>
      <div class="dropdown-controls">
        <div class="dropdown-wrapper">
          <!-- icon goes here -->
          <span class="material-symbols-outlined navbar-icon">font_download</span>
          <div class="select-border"><select name="theme-selector" id="theme-selector" class="navbar-input"
              @change="emit('changeTheme', $event.target.value)">
              <option v-for="theme in themes" :value="theme.value">{{ theme.text }}
              </option>
            </select></div>
        </div>
        <div class="dropdown-wrapper">
          <!-- icon goes here -->
          <span class="material-symbols-outlined navbar-icon">imagesearch_roller</span>
          <div class="select-border"><select name="theme-selector" id="theme-selector" class="navbar-input"
              @change="emit('changeTheme', $event.target.value)">
              <option v-for="theme in themes" :value="theme.value">{{ theme.text }}
              </option>
            </select></div>
        </div>
      </div>
    </div>
  </nav>
</template>
  
<script setup>
import { ref } from 'vue';

const props = defineProps(['stories', 'selectedStoryIndex', 'showNavbar'])

// --- Outbound events: ---
const emit = defineEmits(['openDialogue', 'selectStory', 'deleteStory', 'changeTheme', 'toggleNavbar'])
const openImporter = function () {
  emit("openDialogue", "importNewImage")
}

const selectStory = function (index) {
  emit("selectStory", index)
}

const toggleNavbar = function () {
  emit("toggleNavbar")
}

// --- Theme selection: ---
const themes = ref([
  {
    text: 'Yoitsuba-B',
    value: 'yoitsuba-b'
  },
  {
    text: 'Yoitsuba',
    value: 'yoitsuba'
  },
  {
    text: 'Tomorrow',
    value: 'tomorrow'
  },
])

// --- Page button class management: ---
const getButtonClass = function (index) {
  if (props.selectedStoryIndex === index) {
    return 'selected'
  } else {
    return ''
  }
}

// --- Story selection/deletion: ---
const onStoryClick = function (index) {
  selectStory(index)
}

const onDeleteStory = function (index) {
  emit('deleteStory', index)
}
</script>

<style scoped></style>