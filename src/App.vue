<template>
  <div id="app">
    <TestPage />
  </div>
</template>

<script>
import TestPage from './components/TestPage.vue'

export default {
  name: 'App',
  components: {
    TestPage
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@800;900&display=swap');

#app {
  background-color: #eee;
  min-height: 100vh;
}

</style>
