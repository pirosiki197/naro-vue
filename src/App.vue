<script setup lang="ts">
import ClickCounter from './components/ClickCounter.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <ClickCounter />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>
