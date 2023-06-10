<script setup lang="ts">
import TodoList from './components/TodoList.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <TodoList />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>
