<template>
  <div v-if='results'>
    <div class='row'>
      <div class='col'>
        <h2 class='results-heading'>Results</h2>
        <p>Congratulations! You have a typing speed of {{ results.wpm }} WPM.</p>
      </div>
      <div class='col'>
        <h2 class='results-heading'>Mistyped Words</h2>
        <ol>
          <li v-for='(word, i) in results.mistakeWords' :key='i'>{{ word }}</li>
        </ol>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TestResults',
  props: ['results']
}
</script>

<style scoped>

.results-heading {
  font-family: 'Montserrat', sans-serif;
  font-weight: 900;
}

</style>