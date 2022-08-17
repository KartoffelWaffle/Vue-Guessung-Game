<template>
    <div>
        <h1 class="word">{{this.output}}</h1>
    </div>
</template>

<script>

import data from '../assets/data.json'

const axios = require('axios').default;
const { v4: uuidv4 } = require('uuid');

export default {
  name: 'WordGenerator',
  props: {
    sentOutput: '',
    clear: false,
  },
  data () {
    return {
        generatedWord: String,
        generatedLanguage: String,
        words: data.Words,
        languages: data.Languages,
        translatedWord: "",
        output: ""
    }
  },
  mounted() {
    this.BuildTranslation();
  },
  methods: {
    NewWord() {
        return this.words[Math.floor(100 * Math.random() + 1)];
    },
    NewLanguage() {
        return this.languages[Math.floor(21 * Math.random() + 1)];
    },
    NewTranslation() {
        return this.Translate();
    },
    async BuildTranslation() {
        this.generatedWord = await this.NewWord();
        this.generatedLanguage = await this.NewLanguage();
        this.translatedWord = await this.NewTranslation();
        this.SendTranslation();
    },
    SendTranslation() {
        this.$emit('translationInfo', {
            'Translation': this.translatedWord, 
            'OriginalWord': this.generatedWord,
            'Language': this.generatedLanguage.Name
        });
    },
    Translate () {

        const req = axios({
            baseURL: 'https://api.cognitive.microsofttranslator.com',
            url: '/translate',
            method: 'post',
            headers: {
                'Ocp-Apim-Subscription-Key': 'Subscription_KEY',
                'Ocp-Apim-Subscription-Region': 'REGION',
                'Content-type': 'application/json',
                'X-ClientTraceId': uuidv4().toString()
            },
            params: {
                'api-version': '3.0',
                'from': 'en',
                'to': [this.generatedLanguage.AzureName]
            },
            data: [{
                'text': this.generatedWord
            }],
            responseType: 'json'
        }).then(function(response){
            return JSON.parse(JSON.stringify(response.data[0].translations[0].text, null, 4));
        }).catch((error) => {
            console.log(error)
            return "Oh, it looks like something went wrong.  Try refreshing.";
        })

        return req;
    },
    AlterOutput(newOutput) {
        this.output = newOutput;
    }
  },
  watch: {
    sentOutput(newOutput) {
        this.AlterOutput(newOutput);
    },
    translatedWord(newOutput) {
        this.AlterOutput(newOutput);
    },
    clear(newValue) {
        if (newValue) {
            this.BuildTranslation();
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

div {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    justify-content: center;
}

.word {
    color: white;
    grid-column: 2;
}
</style>
