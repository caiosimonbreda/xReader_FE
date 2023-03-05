<template>
  <nav class="navbar">
    <div class="nav-items-left">
      <h1 class="logo-text">/x/Reader</h1>
      <select name="theme-selector" id="theme-selector" class="navbar-input">
        <option v-for="theme in themes" :value="theme.value">{{ theme.text }}
        </option>
      </select>
    </div>
    <div class="nav-items-right">
      <button v-for="story, index in props.stories" class="navbar-input" @click="selectStory(index)">{{ index+1 }}</button>
      <button class="navbar-input" id="add-stories-btn" @click="openImporter">+</button>
    </div>
  </nav>
</template>
  
<script setup>
import { ref, watch } from 'vue';

const props = defineProps(['stories'])

// --- Outbout events: ---
const emit = defineEmits(['openDialogue', 'selectStory'])
const openImporter = function() {
  emit("openDialogue", "importNewImage")
}

const selectStory = function(index) {
  emit("selectStory", index)
}

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
    text: 'Photon',
    value: 'photon'
  },
])



</script>

<style scoped>
.navbar {
  background-color: #181818;
  height: 58px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 1em;
  padding-right: 0.25em;
  padding-block: 10px;
}

.nav-items-left {
  display: flex;
  flex-direction: row;
  height: 100%;
  align-items: center;
}

.nav-items-right {
  display: flex;
  flex-direction: row;
  height: 100%;
  align-items: center;
  justify-content: flex-end;
  max-width: 73%;
  overflow-x: auto;
}

.logo-text {
  font-family: Arial, Helvetica, sans-serif;
  margin-right: 1em;
  color: #829F5E;
  font-weight: lighter;
  font-size: 1.2em;
}

.navbar-input {
  background-color: #2f2f2f;
  color: #a6a6a6;
  height: 100%;
  border-radius: 5px;
  border: none;
}

select.navbar-input {
  padding-left: 1em;
  border-right: 1em solid transparent;
  padding-right: 0.8em;
}

button.navbar-input {
  min-width: 38px;
  width: 38px;
  margin-inline: 5px;
}

button.navbar-input:hover,
button.navbar-input:active {
  background-color: #3f3f3f;
}

#add-stories-btn {
  width: 38px;
  font-size: 1.15em;
}
</style>


  