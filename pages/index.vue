<template>
  <div>
    <Header 
    :num-correct="numCorrect"
    :num-total="numTotal"
    />
    <QuestionBox
    v-if="questions.length"
    :current-question="questions[index]"
    :next="next"
    :increment="increment"
    />
  </div>
</template>

<script>
  export default {
    name:"IndexPage",
    data(){
      return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0
      }
    },
    mounted(){
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
        method: 'get'
      })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      })
    },
    methods: {
      next(){
        this.index++
      },
      increment(isCorrect){
        if(isCorrect){
          this.numCorrect++
        }
        this.numTotal++
      }
    }
  }
</script>

<style scoped>

</style>
