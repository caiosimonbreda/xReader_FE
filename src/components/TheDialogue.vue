<template>
  <div class="dialogue-outer-wrapper">
    <div class="dialogue-header-wrapper">
      <h3 class="dialogue-title">
        <slot name="title">Dialog title</slot>
      </h3>
      <button class="dialogue-close-button" @click="closeDialogue">✕</button>
    </div>
    <div class="dialogue-content-wrapper">
      <slot name="content">
        <p>Lorem ipsum dolor sit amet lmao even</p>
      </slot>
    </div>
  </div>
</template>

<script setup>
  const emit = defineEmits(['closeDialogue'])

  const closeDialogue = () => {
    emit("closeDialogue")
  }
</script>

<style scoped>
.dialogue-outer-wrapper {
  display: flex;
  flex-direction: column;
  font-family: Arial, Helvetica, sans-serif;
  color: white;
  background-color: #181818;
  border-radius: 7px;
  padding: 1em;
}

.dialogue-header-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding-bottom: 1.6em;
}

.dialogue-title {
  color: #a6a6a6;
  font-weight: normal;
  font-size: 1.2em;
}

.dialogue-close-button {
  font-size: 0.8em;
  background-color: transparent;
  border-radius: 5px;
  color: white;
  border: none;
  width:20px;
  height: 20px;
  /* line-height: 23px; */
}

.dialogue-close-button:hover {
  background-color: #444;
}</style>