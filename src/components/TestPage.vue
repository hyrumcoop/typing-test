<template>
  <div class="container page-container">
    <h1 class='page-heading'>Typing Test</h1>
    <typing-test @oncomplete='onComplete' />
    <test-results :results='results' />
  </div>
</template>

<script>
import TypingTest from './TypingTest.vue';
import TestResults from './TestResults.vue';

export default {
  name: 'TestPage',
  components: {
    TypingTest, TestResults
  },
  data() {
    return {
      results: null
    }
  },
  methods: {
    onComplete(results) {
      this.results = results;
    }
  }
}
</script>

<style scoped>

.page-container {
  min-height: 100vh;
  background-color: #fff;
  box-shadow: 0px 0px 5px 2px #ccc;
}

.page-heading {
  font-family: 'Montserrat', sans-serif;
  font-weight: 900;
}

</style>
