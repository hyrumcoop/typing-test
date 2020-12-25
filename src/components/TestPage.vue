<template>
  <div class="container page-container">
    <h1>Typing Test</h1>
    <typing-test />
  </div>

</template>

<script>
import TypingTest from './TypingTest.vue'

export default {
  name: 'TestPage',
  components: {
    TypingTest
  }
}
</script>

<style scoped>

.page-container {
  min-height: 100vh;
  background-color: #fff;
  box-shadow: 0px 0px 5px 2px #ccc;
}

</style>
