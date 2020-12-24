<template>
  <div class='typing-test'>
    <p>WPM: {{ totalWPM }}</p>
    <p>CPM: {{ totalCPM }}</p>
    <p>{{ passage }}</p>
    <textarea
      v-model='input'
      @keydown='onKeyPress'
      @input='onType'
      class='test-textarea'
      :class='{error: containsError, complete: isComplete}'
    />
    <button @click='reset()'>Reset</button>
  </div>
</template>

<script>
import Passages from '../passages';

const ALLOWED_INPUT_TYPES = ['insertText', 'deleteContentBackward'];

const randomPassage = () => {
  let index = Math.floor(Math.random() * Passages.length);
  return Passages[index]
}

const initialState = (passage) => {
  if (!passage) passage = randomPassage().passage;

  return {
    began: false,

    passage,
    input: '',
    prevInput: '',
    beginTime: null,
    finishTime: null,

    isComplete: false,
    containsError: false,

    curWordIndex: 0,
    wordBeginTime: null,
    wordTimes: [],
    mistakes: {} // dict of arrays for each each index; each array contains all incorrect spellings typed by the user
  }
}

export default {
  name: 'TypingTest',
  data() {
    return initialState()
  },
  props: {},
  methods: {
    onKeyPress(event) {
      // prevent user from backspacing a correct word
      if (this.isCorrect() && event.key == 'Backspace') return event.preventDefault();

      if (this.isComplete) return event.preventDefault();
    },
    onType(event) {
      if (!this.began) this.onBegin();

      // prevent user from copy/pasting
      if (this.isIllegalInput(event)) {
        return this.input = this.prevInput;
      } else {
        this.prevInput = this.input;
      }

      this.containsError = !this.isCorrect();

      // mark word as an error if typed incorrectly
      if (this.containsError) {
        this.recordError();
      }

      // increase word index when space is typed following a correct sequence of words
      if (event.data == ' ' && !this.containsError) {
        this.wordTimes[this.curWordIndex] = Date.now() - this.wordBeginTime;
        this.curWordIndex++;
      }

      if (!this.isComplete && this.passage == this.input) {
        this.onComplete();
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
    onBegin() {
      this.began = true;
      this.beginTime = Date.now();
      this.wordBeginTime = Date.now();
    },
    onComplete() {
      this.isComplete = true;
      this.finishTime = Date.now();

      this.$emit('oncomplete', {
        passage: this.passage,
        elapsedMinutes: this.totalMinutesElapsed(this.beginTime, this.finishTime),
        mistakeCount: this.mistakeCount,
        mistakes: this.mistakes,
        wordTimes: this.wordTimes
      })
    },
    isIllegalInput(event) {
      if (!ALLOWED_INPUT_TYPES.includes(event.inputType)) return true;

      // check if more than one character was deleted or inserted
      let lenDiff = Math.abs(this.input.length - this.prevInput.length);
      if (lenDiff != 1) return true;
    },
    totalMinutesElapsed(start, end) {
      if (!start || !end) return (Date.now() - this.beginTime) / (60 * 1000)

      return (end - start) / (60 * 1000);
    },
    reset() {
      Object.assign(this.$data, initialState());
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
    },
    totalWPM() {
      return Math.floor(this.wordsComplete / this.totalMinutesElapsed());
    },
    totalCPM() {
      return Math.floor(this.charactersComplete / this.totalMinutesElapsed());
    },
    mistakeCount() {
      return Object.keys(this.mistakes).length;
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