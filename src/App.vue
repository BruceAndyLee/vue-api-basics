<template>
  <div id="app">
    <Header 
      :correctCtr="correctCtr"
      :totalCtr="totalCtr"
    />
    <QuestionBox
      v-if="questions.length"
      :index="index"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
    />
  </div>
</template>

<script>

import Header from './components/Header'
import QuestionBox from './components/QuestionBox'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctCtr: 0,
      totalCtr: 0
    }
  },
  methods: {
    next() {
      if (this.index === this.questions.length - 1) {
        this.index = 0;
      } else {
        this.index++;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctCtr++;
      }
      this.totalCtr++;
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
    .then(resp => {
      return resp.json()
    })
    .then(jsonData => {
      this.questions = jsonData.results;
      console.log(this.questions);
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
