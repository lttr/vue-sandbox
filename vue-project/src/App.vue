<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>
  <main>
    <RouterView />
  </main>
</template>

<style scoped>
.wrapper {
  display: flex;
}

nav {
  display: flex;
  gap: 1rem;
  margin-inline: auto;
}

main {
  height: 100%;
  display: grid;
  place-items: center;
}
</style>
