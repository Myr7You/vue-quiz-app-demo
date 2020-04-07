<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row align-h="center">
        <b-col sm="6">
          <b-alert :show="numTotal === 10" variant="success">
            <hr />
            <h4 class="alert-heading">Well done!</h4>
            <p>
              You have finish all the question.
              {{ numCorrect }} right!
            </p>
            <hr />
            <p class="mb-0" v-if="numCorrect > 7">
              Congratulation!
            </p>
          </b-alert>
        </b-col>
      </b-row>
      <b-row align-h="center">
        <b-col sm="6">
          <QuestionBox
            v-if="question.length"
            :quest="question[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      question: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      if (this.index < 10) {
        this.index++;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsondata) => {
        this.question = jsondata.results;
      });
  },
};
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
.alert {
  margin-top: 15px;
}
</style>
