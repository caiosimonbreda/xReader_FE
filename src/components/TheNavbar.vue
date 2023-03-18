<template>
  <nav class="navbar">
    <div class="nav-items-left">
      <h1 class="logo-text">/x/Reader</h1>
      <select name="theme-selector" id="theme-selector" class="navbar-input" @change="emit('changeTheme', $event.target.value)">
        <option v-for="theme in themes" :value="theme.value">{{ theme.text }}
        </option>
      </select>
    </div>
    <div class="nav-items-right">
      <button 
        v-for="story, index in props.stories" 
        :class="`navbar-input ${getButtonClass(index)}`"
        @click="onMouseUp(index)"
        @mousedown="startClickTimer(index)"
        @mouseout="cancelClickTimer(index)"
        >
        {{ showDeleteActionForIndex !== index ? index+1 : 'DEL'}}
      </button>
      <button 
        class="navbar-input" 
        id="add-stories-btn" 
        @click="openImporter"
      >+</button>
    </div>
  </nav>
</template>
  
<script setup>
import { ref, computed } from 'vue';

const props = defineProps(['stories', 'selectedStoryIndex'])

// --- Outbound events: ---
const emit = defineEmits(['openDialogue', 'selectStory', 'deleteStory', 'changeTheme'])
const openImporter = function() {
  emit("openDialogue", "importNewImage")
}

const selectStory = function(index) {
  emit("selectStory", index)
}

// --- Theme selection: ---
const themes = ref([
  {
    text: 'Tomorrow',
    value: 'tomorrow'
  },
  {
    text: 'Yoitsuba',
    value: 'yoitsuba'
  },
  {
    text: 'Yoitsuba-B',
    value: 'yoitsuba-b'
  },
])

// --- Page button class management: ---
const getButtonClass = function(index) {
  if (showDeleteActionForIndex.value === index) {
    return 'delete'
  } else if (props.selectedStoryIndex === index) {
    return 'selected'
  } else {
    return ''
  }
}


// --- Long press handling: ---
const clickTimer = ref(null)
const longClick = ref(false)

const showDeleteActionForIndex = ref(null);

const startClickTimer = function(index) {
  clickTimer.value = setTimeout(() => {
    longClick.value = true
    //add delete class to page button
    showDeleteActionForIndex.value = index
  }, 500)
}

const cancelClickTimer = function() {
  longClick.value = false
  showDeleteActionForIndex.value = null
  clearTimeout(clickTimer.value)
  clickTimer.value = null
}

const onMouseUp = function(index) {
  if (!longClick.value) {
    selectStory(index)
  } else {
    emit('deleteStory', index)
  }
  showDeleteActionForIndex.value = null
  clearTimeout(clickTimer.value)
  clickTimer.value = null
  longClick.value = false
}
</script>

<style scoped>
</style>


  