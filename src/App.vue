<template>

  <!-- header -->
  <header>

    <h1 style="margin: 0; padding: 0;">Hylands <span style="font-size: 0.875rem;">A NeoMUD</span></h1>

  </header>

  <!-- xterm --> 
  <FullscreenXterm />

  <!-- footer -->
  <footer>

    &copy; 2025 Hylands.app

  </footer>

</template>

<script setup lang="ts">

  // imports
  import FullscreenXterm from './components/FullscreenXterm.vue'

</script>

<style lang="scss">
 
  // header
  header {

  }

  // footer
  footer {
    text-align: center;
  }

</style>
