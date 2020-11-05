<template>
  <div class="quiz">
    <Header v-bind:numTotal="numTotal" v-bind:numCorrect="numCorrect" />
    <QuizCard v-bind:question="question" v-on:next-question="nextQuestion" v-on:reset-game="resetGame" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuizCard from "./components/QuizCard.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    QuizCard,
  },
  data() {
    return {
      question: {},
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    nextQuestion(correct) {
      this.numTotal++;
      if (correct) {
        this.numCorrect++;
      }
      this.loadNewQuestion();
    },
    loadNewQuestion() {
      axios
        .get("https://opentdb.com/api.php?amount=1&category=27&type=multiple")
        .then((res) => {
          this.question.question = res.data.results[0].question;
          this.question.correct = Math.floor(Math.random() * 4);
          this.question.answers = [...res.data.results[0].incorrect_answers];
          this.question.answers.splice(
            this.question.correct,
            0,
            res.data.results[0].correct_answer
          );
        })
        .catch((err) => console.log(err));
    },
    resetGame() {
      this.loadNewQuestion();
      this.numCorrect = 0;
      this.numTotal = 0;
    },
  },
  created() {
    this.loadNewQuestion();
  },
};
</script>

<style>
#app, body {
  font-family: Arial;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.quiz {
  width: 600px;
  max-width: 80%;
  border: 2px solid grey;
  border-radius: 5px;
  padding: 10px;
}
</style>
