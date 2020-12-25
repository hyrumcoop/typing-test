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

#app {
  background-color: #eee;
  min-height: 100vh;
}

</style>
