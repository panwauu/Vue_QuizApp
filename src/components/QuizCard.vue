<template>
    <div class="card-body">
    <h2>{{ question.question }}</h2>
    <div class="AnswerContainer">
      <button
        class="AnswerButton"
        v-for="(answer, id) in question.answers"
        :key="id"
        v-bind:class="{
          correct: selected !== -1 && question.correct === id,
          incorrect: selected === id && question.correct !== selected,
        }"
        v-on:click="selectAnswer(id)"
      >
        {{ answer }}
      </button>
    </div>
    <button class="SubmitButton" v-on:click="$emit('next-question', (selected === question.correct)); selected = -1;" :disabled="selected === -1">Next Question</button>
    </div>
    <button v-on:click="$emit('reset-game'); selected = -1;">Reset</button>
</template>

<script>
export default {
  name: "QuizCard",
  props: ["question"],
  data: function () {
    return {
      selected: -1,
    };
  },
  methods: {
    selectAnswer: function (id) {
      if(this.selected === -1) {this.selected = id;}
    },
  },
};
</script>

<style scoped>
.AnswerContainer {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.SubmitButton {
  display: flex;
}

.correct {
  background-color: green;
}

.incorrect {
  background-color: red;
}
</style>