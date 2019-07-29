<template>
 <b-jumbotron>
    <div v-html="currentQuestion.question">
      
    </div>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item
      @click="selectAnswer(index)"
      v-for="(answer, index) in shuffledAnswers"
      :key="index"
      :class="[!answered && selectedIndex === index ? 'selected' : 
      answered && correctIndex === index ? 'correct' :
      answered && selectedIndex === index ? 'incorrect' : '']"
      >
      {{ answer }}
      </b-list-group-item>
    </b-list-group>

    <b-button
    variant="primary"
    @click="submitAnswer()"
    :disabled="selectedIndex === null || answered"
    href="#">
      Submit
    </b-button>

    <b-button 
    @click="next" 
    variant="success" 
    href="#">
      Next
    </b-button>

  </b-jumbotron>
</template>

<script>

import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]; // making copy of array, not referencing the same array
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.answered = true;

      this.increment(isCorrect);
    }
  }
}
</script>

<style scoped>
  .list-group {
    padding-bottom: 2em;
  }

  .list-group-item:hover {
    background-color: cyan;
    cursor: pointer;
  }

  .btn {
    margin-right: 1em;
  }

  .selected {
    background-color: cadetblue;
  }

  .correct {
    background-color: green;
  }

  .incorrect {
    background-color: red;
  }
</style>


