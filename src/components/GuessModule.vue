<template>
  <div class="guess-area">
    <div class="grid1">
      <input class="guess-text" placeholder="Guess the word" v-model="wordGuess">
    </div>
    <div class="grid2">
      <select class="guess-language" v-model="languageGuess">
        <option value="" selected disabled>Guess the language</option>
        <option v-for="language in languages" :value="language.Name" :key="language.AzureName">{{ language.Name }}</option>
      </select>
      <button class="submit-guess" @click="SubmitGuess">Make Guess!</button>
    </div>
  </div>
</template>

<script>

import data from '../assets/data.json';

export default {
  name: 'GuessModule',
  props: {
    clear: false
  },
  data () {
    return {
        words: data.words,
        languages: data.Languages,
        wordGuess: "",
        languageGuess: ""
    }
  },
  methods: {
    SubmitGuess() {
      this.$emit('guess', {
        'word': this.wordGuess,
        'language': this.languageGuess
      });
    }
  },
  watch: {
    clear(value) {
      if(value) {
        this.wordGuess = '';
        this.languageGuess = "";
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.guess-area {
  display: grid;
  justify-content: center;
  grid-template-columns: 1fr 1fr 1fr;
  padding: 5%;
}

.grid1 {
  grid-column: 2;
  grid-row: 1;
}

.grid1, .grid2 {
  box-sizing: border-box;
  justify-content: center;
  margin: 1% 0;
}

.grid2 {
  grid-column: 2;
  grid-row: 2;
}

.guess-text, .guess-language {
  width: 100%;
  font-size: large;
  border: 1px solid rgba(68, 78, 95, 1);
  border-radius: 5px;
  color: #fff;
  height: 35px;
  background-color: transparent;
  padding: 2px 6px;
  box-sizing: border-box;
  display: block;
  margin: 5px 0;
}

/* Adjustments for mobile devices */

@media only screen and (max-width: 600px){
  .guess-area {
    grid-template-columns: 1fr 4fr 1fr;
  }
}

/* End of mobile Adjustments */

.guess-language > option {
  background-color: #2B303A; 
}

::placeholder {
  opacity: 1;
  color: white;
}

.submit-guess {
  margin: 20% 0;
  width: 20%;
  font-size:15px;
  font-family:Arial;
  width:140px;
  height:50px;
  color:#fff;
  border: 1px solid rgba(68, 78, 95, 1);
  font-weight:bold;
  border-radius: 6px;
  text-shadow: 1px 1px 0px rgba(60, 62, 75, 1);
  background: transparent;
}

.submit-guess:hover {
  background: rgba(68, 78, 95, 1)
}

.submit-guess:active {
  background: #2B303A;
}

</style>
