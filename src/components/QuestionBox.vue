<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{ currentQuestion.question }}</template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >Enviar</b-button>
      <b-button @click="next" variant="success" href="#">Proxima questao</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);

      return answers;
    }
  },
  //similar to computed and methods, but Watch method receive Objects or functions, and wait for changes on the props to run this function
  watch: {
    //Turn currentQuestion to a object that, with the attr immediate, so when render for the first time it execute the handler
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null; //Erase previous answers index when load a new question.
        this.answered = false; //Reset if the answers was already submit
        this.shuffleAnswers();
      }
    }
    // (){
    //   this.selectedIndex = null //Erase previous answers index when load a new question.
    //   this.shuffleAnswers()
    // }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);

      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index) { //Mark the selected answer
        answerClass = 'selected'
      } else if ( this.answered && this.correctIndex === index) { //If the selected answer is correct 
        answerClass = 'correct'
        
        //If the selected answer is not correct 
      } else if (this.answered && 
        this.selectedIndex === index && 
        this.correctIndex !== index
      ){
        answerClass = 'incorrect'
      }

      return answerClass
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
  border-radius: 0px;
}

.list-group-item:hover {
  background-color: lightgray;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
  border-color: green;
}

.incorrect {
  background-color: lightcoral;
  border-color: red;
}
.btn {
  margin: 0 5px;
}
</style>