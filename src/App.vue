<script setup>
import TodoApp from "./components/TodoApp.vue";
</script>

<template>
  <div class="main_bg">
    <TodoApp />
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.main_bg {
  width: 100vh;
  padding: 10px 2px;
  border: 2px solid white;
  border-radius: 10px;
}

@media (min-width: 1024px) {
  /* header {
  } */
}
</style>
