<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <h3 v-html="quest.question"></h3>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAns(index)"
          :class="ansClass(index)"
          :disabled="answered"
          v-html="answer"
        >
        </b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submitAns"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
      <b-button
        variant="success"
        :disabled="selectedIndex === null || !answered || numTotal === 10"
        @click="next"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    quest: Object,
    numTotal: Number,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false,
    };
  },
  computed: {
    // eslint-disable-next-line vue/return-in-computed-property
    answers() {
      let answers = this.quest.incorrect_answers;
      answers.push(this.quest.correct_answer);
      return answers;
    },
  },
  watch: {
    quest: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.correctIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
    //change to object and immediate true make it change the first time when the data pass to quest
    // quest() {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // },
  },
  methods: {
    selectAns(index) {
      this.selectedIndex = index;
      console.log(index);
    },
    submitAns() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.quest.incorrect_answers,
        this.quest.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.quest.correct_answer
      );
    },
    ansClass(index) {
      let ansClass = "";
      if (!this.answered && this.selectedIndex === index) {
        ansClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        ansClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        ansClass = "incorrect";
      }
      return ansClass;
    },
  },
  mounted() {
    // console.log(this.quest);
  },
};
</script>

<style scoped>
.jumbotron {
  margin-top: 20px;
}
.list-group {
  margin-bottom: 15px;
}
.list-group-item {
  color: #666;
}
.list-group-item:hover {
  background-color: #cce5ff;
  cursor: pointer;
  font-weight: bold;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: #cce5ff;
  font-weight: bold;
}
.correct {
  background-color: #28a745;
  color: #fff;
  font-weight: bold;
}
.incorrect {
  background-color: #f8d7da;
  color: #dc3545;
  font-weight: bold;
}
</style>
