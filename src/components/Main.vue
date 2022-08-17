<template>
  <div>
    <ScoreTracker :addToScore="this.score"/>
    <WordGenerator :clear="this.newWordFlag" :sentOutput="this.answerMessage" @translationInfo="SetTranslationInfo"/>
    <GuessModule :clear="this.clearGuessFlag" @guess="SetGuess" />
  </div>
</template>

<script>

import GuessModule from './GuessModule.vue'
import WordGenerator from './WordGenerator.vue'
import ScoreTracker from './ScoreTracker.vue'

export default {
  name: 'Main',
  components: {
    GuessModule,
    WordGenerator,
    ScoreTracker
  },
  data() {
    return {
      score: 0,
      translatedWord: String,
      originalWord: String,
      selectedLanguage: String,
      guessedLanguage: String,
      guessedWord: String,
      answerMessage: String,
      newWordFlag: false,
      clearGuessFlag: false,
      tryCount: 0
    }
  },
  methods: {
    SetTranslationInfo(info) {
      this.translatedWord = info.Translation;
      this.originalWord = info.OriginalWord;
      this.selectedLanguage = info.Language;
    },
    SetGuess(guess) {
      this.guessedLanguage = guess.language;
      this.guessedWord = guess.word;
    },
    CompareAnswers() {
      if(this.tryCount < 3) {
        if(this.guessedWord.toLowerCase() === this.originalWord.toLowerCase() && this.guessedLanguage === this.selectedLanguage) {
          this.answerMessage = 'Congrats you\'ve got the right answer!!!'
          this.score += 1;
          setTimeout(() => {
            this.answerMessage = ''
            this.clearGuessFlag = true;
            this.newWordFlag = true;
          }, 2500);
          this.clearGuessFlag = false;
          this.newWordFlag = false;
          this.tryCount++;
        }
        else if(this.guessedWord.toLowerCase() === this.originalWord.toLowerCase() || this.guessedLanguage === this.selectedLanguage) {
          this.answerMessage = 'You\'re half way there!  Give it another go.';
          setTimeout(() => {
            this.answerMessage = this.translatedWord;
          }, 2500);
          this.tryCount++;
        }
        else {
          this.answerMessage = 'Sorry, that\'s not quite right.  Try again.';
          setTimeout(() => {
            this.answerMessage = this.translatedWord;
          }, 2500);
          this.tryCount++;
        }
      }
      if(this.tryCount === 3) {
        this.answerMessage = `Sorry you\'ve ran out of guesses!  The correct word was ${this.originalWord} and the correct language was ${this.selectedLanguage}`;
          this.score += 1;
          setTimeout(() => {
            this.answerMessage = ''
            this.clearGuessFlag = true;
            this.newWordFlag = true;
          }, 3500);
          this.clearGuessFlag = false;
          this.newWordFlag = false;
      }
    }
  },
  watch: {
    guessedWord(guess, oldGuess) {
      this.CompareAnswers();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
