<template>
  <div v-if='results'>
    <div class='row'>
      <div class='col'>
        <h2 class='results-heading'>Results</h2>
        <p>Congratulations! You typed at a speed of <span class='wpm'>{{ results.wpm }} WPM</span>
            on this passage (<span class='cpm'>{{ results.cpm }} CPM</span>). Your accuracy per word
            on this passage is <span class='accuracy'>{{ results.percAccuracy }}%</span>.</p>
        <span class='shareText'>Share your score to Twitter!</span>
        <twitter-button :description='twitterDesc' btnText />
      </div>
      <div class='col'>
        <h2 class='results-heading'>Mistyped Words</h2>
        <p>Below are the words that you mistyped, along with the error you typed or began to type.</p>
        <b-table :items='mistakeItems' :fields='mistakeFields' />
      </div>
    </div>
  </div>
</template>

<script>
import { BTable } from 'bootstrap-vue';
import TwitterButton from "vue-share-buttons/src/components/TwitterButton";

export default {
  name: 'TestResults',
  components: { BTable, TwitterButton },
  props: ['results'],
  data() {
    return {
      mistakeFields: [
        {
          key: 'word',
          label: 'Word'
        },
        {
          key: 'misspelling',
          label: 'Your Misspelling',
          variant: 'danger'
        }
      ]
    }
  },
  computed: {
    mistakeItems() {
      return this.results.mistakeWords.map(word => {
        return {
          word,
          misspelling: this.results.misspellings[word].reduce((longest, cur) => cur.length > longest ? cur : longest, '')
        }
      });
    },
    twitterDesc() {
      return 'I type ' + this.results.wpm + ' WPM with ' + this.results.percAccuracy + '% accuracy per word. Can you beat me?';
    }
  }
}
</script>

<style scoped>

.results-heading {
  font-family: 'Montserrat', sans-serif;
  font-weight: 900;
}

p {
  font-size: 18px;
}

.shareText {
  font-family: 'Montserrat', sans-serif;

  font-size: 18px;
  font-weight: bold;

  line-height: 44px;
}

.wpm, .cpm, .accuracy {
  font-weight: bold;
  padding: 2px 5px;
}

.wpm {
  background-color:  #81c784;
}

.cpm {
  background-color: #64b5f6;
}

.accuracy {
  background-color: #ef5350;
}

</style>