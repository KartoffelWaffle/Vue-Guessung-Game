<template>
    <div>
        <h3 class="score">Score: {{this.score}}</h3>
    </div>
</template>

<script>

export default {
  name: 'ScoreTracker',
  props: {
    addToScore: Number
  },
  data() {
    return {
        score: 0
    }
  },
  watch: {
    addToScore(newScore) {
        this.score = newScore;
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

.score {
    color: white;
    grid-column: 2;
}

</style>
