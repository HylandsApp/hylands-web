<template>

  <!-- header -->
  <header>

    <!-- nav -->
    <nav>

      <!-- link -->
      <RouterLink to="/">Home</RouterLink>

      <!-- link -->
      <RouterLink to="/about">About</RouterLink>

    </nav>

  </header>

  <!-- router view --> 
  <RouterView />

</template>

<script setup lang="ts">

  // imports
  import { RouterLink, RouterView } from 'vue-router'

</script>

<style lang="scss">
  // style here
</style>
