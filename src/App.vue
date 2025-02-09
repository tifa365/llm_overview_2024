<script setup>
import Timeline from './components/Timeline.vue';
</script>

<template>
  <Timeline />
</template>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  color: #333;
  margin: 0;
  padding: 0;
  overflow-x: scroll;
  scrollbar-width: thin;
  -ms-overflow-style: -ms-autohiding-scrollbar;
}

/* Scrollbar styles */
body::-webkit-scrollbar {
  height: 8px;
}

body::-webkit-scrollbar-track {
  background: #f1f1f1;
}

body::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 4px;
}

body::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>