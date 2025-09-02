<template>
  <div id="app-container">
    <h1>Vue 3 + PixiJS + DragonBones</h1>
    <DragonBonesAvatar />
  </div>
</template>

<script setup lang="ts">
import DragonBonesAvatar from './components/DragonBonesAvatar.vue';
</script>

<style>
#app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  background-color: #f0f0f0;
}

canvas {
  border: 1px solid #ccc;
}

h1 {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}
</style>
