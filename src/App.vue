<template>
  <div id="app" :style="{backgroundColor: color}">
    <Header
      :logo="logo"
    />
    <QuestionBox
      :questions="questions.results"
      :currentQuestion="questions.results[index]"
      :nextQuestion="nextQuestion"
      :totalNumQuestions="totalNumQuestions"
      :questionCount="index + 1"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      index: 0,
      questions: [],
      totalNumQuestions: 0,
      color: '#124DF0',
      logo: 'FilmQuiz'
    }
  },
  methods: {
    nextQuestion() {
      this.index++;
    },
    getTotalNumQuestions() {
      let questions = this.questions.results;
      this.totalNumQuestions = questions.length;
    }
  },

  async mounted() {
    await axios.get('https://opentdb.com/api.php?amount=10&category=11')
      .then(function(response) {
        this.questions = response.data;
      }.bind(this))

    this.getTotalNumQuestions();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Varela+Round&display=swap');

html, body {
  height: 100%;
}

#app {
  font-family: 'Varela Round', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: block;
  height: 100%;
  color: #000;
}
</style>
