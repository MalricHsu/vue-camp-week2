<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <nav>
    <RouterLink to="/review" class="nav">Review</RouterLink>
    <RouterLink to="/todo" class="nav">Todo</RouterLink>
  </nav>

  <RouterView />
</template>

<style scoped>
.nav {
  font-size: 20px;
  margin-right: 12px;
  font-weight: 700;
  color: gray;
}
</style>
