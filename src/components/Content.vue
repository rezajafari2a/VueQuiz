<template>
  <div>
    <b-jumbotron>
      <template #header>Question</template>
      <hr class="my-4" />
      <p>{{ cQuestion.question }}</p>
      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, i) in shuffleAnswers"
          :key="i"
          @click="ChangeIndex(i)"
          :class="classname(i)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        href="#"
        @click="submitAnswer"
        :disabled="cindex === null || answerd"
        >Sumbit</b-button
      >
      <b-button variant="success" href="#" @click="next">next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    cQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      cindex: null,
      shuffleAnswers: [],
      correct_answer: null,
      answerd: false,
    };
  },
  methods: {
    ChangeIndex: function (i) {
      this.cindex = i;
    },
    shuffleAnswersx() {
      let answers = [
        ...this.cQuestion.incorrect_answers,
        this.cQuestion.correct_answer,
      ];
      this.shuffleAnswers = _.shuffle(answers);
      this.correct_answer = this.shuffleAnswers.indexOf(
        this.cQuestion.correct_answer
      );
    },
    submitAnswer() {
      console.log("submitAnswer");
      let is_correct = false;
      if (this.cindex === this.correct_answer) {
        is_correct = true;
      }
      this.answerd = true;
      this.increment(is_correct);
    },
    classname(i) {
      return !this.answerd && i == this.cindex
        ? "selected"
        : this.answerd && i == this.correct_answer
        ? "correct"
        : this.answerd && this.cindex==i && i != this.correct_answer
        ? "incorrect"
        : null;
    },
  },
  watch: {
    cQuestion: {
      immediate: true,
      handler() {
        this.cindex = null;
        this.answerd = null;
        this.correct_answer = null;
        this.shuffleAnswersx();
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.cQuestion.incorrect_answers];
      answers.push(this.cQuestion.correct_answer);
      return answers;
    },
  },
};
</script>
<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 10px 2px;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: lightsalmon;
}
.selected {
  background-color: lightskyblue;
}
</style>