<template>
  <div class="dialogue">
    <div class="dialogue-header-wrapper">
      <h3 class="dialogue-title">
        <slot name="title">Dialog title</slot>
      </h3>
      <button class="dialogue-close-button" @click="closeDialogue">
        <span class="material-symbols-outlined">close</span>
      </button>
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
</style>