<template>
  <div class='typing-test'>
    <p>{{ passage }}</p>
    <textarea
      v-model='input'
      @keypress='onKeyPress'
      @input='onType'
      class='test-textarea'
      :class='{error: !containsError}'
    />
  </div>
</template>

<script>
export default {
  name: 'TypingTest',
  data() {
    return { // TODO: keep track of starting time
      passage: 'This is a difficult typing passage.',
      input: '',
      starttime: null
    }
  },
  props: {},
  methods: {
    onKeyPress() {
      // TODO: prevent user from backspacing a correct word
    },
    onType() {
      // TODO: end timer when passage is done, accumulate errors
    }
  },
  computed: {
    containsError() {
      return this.passage.startsWith(this.input)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.test-textarea {

}

.test-textarea.error {
  background-color:red;
}

</style>