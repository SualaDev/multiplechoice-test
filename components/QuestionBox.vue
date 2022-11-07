<template>
  <div class="main-container">
    <div class="list-items">
      <h1>{{ currentQuestion.question }}</h1>
      <b-list-group>
        <b-list-group-item
        v-for="(answer, index) in answers" 
        :key="index" 
        class="answers"
        :class="answerClass(index)"
        @click.prevent="selectedAnswer(index)"
        >
        {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <div class="button-container">
        <button 
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
        >
        Submit
        </button>
        <button @click="next">Next</button>
      </div>
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import _ from 'lodash'
  export default {
    name:"QuestionBox",
    props: {
      currentQuestion : Object,
      next: Function,
      increment:Function
    },
    data(){
      return {
        selectedIndex : null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false
      }
    },
    computed: {
      answers(){
        const answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return this.shuffledAnswers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler(){
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
    },
    methods: {
      selectedAnswer(index){
        this.selectedIndex = index
      },
      shuffleAnswers(){
        const answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer(){
        let isCorrect = false
        if(this.selectedIndex === this.correctIndex){
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      answerClass(index){
        let answerClass =''
        if(!this.answered && this.selectedIndex === index){
          answerClass = 'selected'
        }
        else if(this.answered && this.correctIndex === index){
          answerClass = 'correct'
        }
        else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
          answerClass = 'incorrect'
        }
        return answerClass
      }
    }
  }
</script>

<style scoped>
.main-container{
  background-color: grey;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  /* width: 1024px; */
}
.list-items{
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 3rem;
}
.list-items h1{
  padding-bottom: 30px;
  border-bottom: 3px solid black;
}
.button-container{
  display: flex;
  column-gap: 2rem;
  margin-bottom: 10px;
}
.button-container button:first-of-type{
  background-color: blue;
  color: #FFF;
  border: none;
  border-radius: 10px;
  padding: 10px;
}
.button-container button:nth-child(2){
  background-color: green;
  color: #FFF;
  border: none;
  border-radius: 10px;
  padding: 10px;
}
.answers:hover{
  background-color: #EEE;
  cursor: pointer;
}
.selected{
  background-color: lightblue;
}
.correct{
  background-color: lightgreen;
}
.incorrect{
  background-color: red;
}
</style>