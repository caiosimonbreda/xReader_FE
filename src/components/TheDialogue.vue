<template>
  <div class="dialogue-outer-wrapper">
    <div class="dialogue-header-wrapper">
      <h3 class="dialogue-title">
        <slot name="title"></slot>
      </h3>
      <button class="dialogue-close-button">X</button>
    </div>
    <div class="dialogue-content-wrapper">
      <slot name="content">
        <p>Lorem ipsum dolor sit amet lmao even</p>
      </slot>
    </div>
  </div>
</template>

<script setup>

</script>

<style scoped>
.dialogue-outer-wrapper {
  display: flex;
  flex-direction: column;
  font-family: Arial, Helvetica, sans-serif;
  color: white;
  background-color: #222;
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
  font-size: 1.2em;
}

.dialogue-close-button {
  background-color: transparent;
  border-radius: 5px;
  color: white;
  border: none;
  width: 22px;
  height: 22px;
  line-height: 23px;
}

.dialogue-close-button:hover {
  background-color: #444;
}</style>