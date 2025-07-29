<template>
  <div id="app">
    <TodoApp />
  </div>
</template>

<script setup lang="ts">
import TodoApp from './components/TodoApp.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}

#app {
  width: 100%;
  max-width: 640px;
}
</style>