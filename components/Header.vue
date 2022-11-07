<template>
  <div class="main-container">
    <h1>Fancy Quiz App</h1>
    <h1>Counter: {{ numCorrect }}/ {{ numTotal }}</h1>
  </div>
</template>

<script>
  export default {
    name:"HeaderPage",
    props: [
      'numCorrect',
      'numTotal'
    ]
  }
</script>

<style scoped>
.main-container{
  display: flex;
  column-gap: 2rem;
  border-bottom: 1px solid grey;
}
</style>