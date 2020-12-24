<template>
  <div class='typing-test'>
    <p>{{ passage }}</p>
    <textarea
      v-model='input'
      @keydown='onKeyPress'
      @input='onType'
      class='test-textarea'
      :class='{error: containsError, complete: isComplete}'
    />
  </div>
</template>

<script>
export default {
  name: 'TypingTest',
  data() {
    return {
      passage: 'This is a difficult typing passage.',
      input: '',
      beginTime: null,

      isComplete: false,
      containsError: false,

      curWordIndex: 0,
      mistakes: {} // dict of arrays for each each index; each array contains all incorrect spellings typed by the user
    }
  },
  props: {},
  methods: {
    onKeyPress(event) {
      // TODO: prevent user from backspacing a correct word

      if (this.isComplete) return event.preventDefault();
    },
    onType(event) {
      if (!this.beginTime) this.beginTime = Date.now(); // TODO: consolidate to its own function

      this.containsError = !this.isCorrect();

      // mark word as an error if typed incorrectly
      if (this.containsError) { // TODO: consolidate to its own function
        this.recordError();
      }

      // increase word index when space is typed following a correct sequence of words
      // TODO: record time for each word
      if (event.data == ' ' && !this.containsError) {
        this.curWordIndex++;
      }

      if (!this.isComplete && this.passage == this.input) {
        this.onComplete(); // TODO: should I use an event instead?
      }
    },
    recordError() {
      let curMistakes = this.mistakes[this.curWordIndex];
      let curWord = this.input.split(' ')[this.curWordIndex];
      // TODO: there's a bug when error contains a space; we need to grab substr without splitting

      if (curMistakes && !curMistakes.includes(curWord)) {
          curMistakes.push(curWord);
      } else if (!curMistakes) {
        this.mistakes[this.curWordIndex] = [curWord]
      }
    },
    isCorrect() {
      return this.passage.startsWith(this.input);
    },
    onComplete() {
      this.isComplete = true;
      console.log('Woohoo!', this.mistakes, this.wordsComplete, this.charactersComplete);

      // TODO: end timer when passage is done, accumulate errors
    }
  },
  computed: {
    // Returns array of words in passage (including punctuation!)
    words() {
      return this.passage.split(' ');
    },

    wordsComplete() {
      return this.curWordIndex + this.isComplete;
    },

    // TODO: research whether spaces should contribute to character count
    charactersComplete() {
      if (this.isCorrect()) return this.input.length;

      let min = this.words.slice(0, this.curWordIndex).join(' ').length;
      return min; // TODO: this is wrong but I'll come back to it later
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.test-textarea {

}

.test-textarea.error {
  background-color: red;
}

.test-textarea.complete {
  background-color: green;
}

</style>