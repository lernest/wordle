<template>
<div v-if="!isEvaluated" class="row">
  <LetterBox :letter="word[0]?word[0]:' '" :color="-1"/>
  <LetterBox :letter="word[1]?word[1]:' '" :color="-1"/>
  <LetterBox :letter="word[2]?word[2]:' '" :color="-1"/>
  <LetterBox :letter="word[3]?word[3]:' '" :color="-1"/>
  <LetterBox :letter="word[4]?word[4]:' '" :color="-1"/>
</div>
<div v-if="isEvaluated" class="row">
  <LetterBox :letter="evaluatedWord[0][0]" :color="evaluatedWord[0][1]" :index="0" />
  <LetterBox :letter="evaluatedWord[1][0]" :color="evaluatedWord[1][1]" :index="1" />
  <LetterBox :letter="evaluatedWord[2][0]" :color="evaluatedWord[2][1]" :index="2" />
  <LetterBox :letter="evaluatedWord[3][0]" :color="evaluatedWord[3][1]" :index="3" />
  <LetterBox :letter="evaluatedWord[4][0]" :color="evaluatedWord[4][1]" :index="4" />
</div>
</template>

<script>
import LetterBox from './LetterBox.vue'
export default {
    props:{
        word: String,
        evaluatedWord: Object,
    },
    components:{
        LetterBox
    },
    computed:{
        isEvaluated(){
            return this.evaluatedWord && !this.word
        }
    }
}
</script>

<style>
@media (max-width: 600px) {
  .row{
    max-width: 95%;
    margin: auto;
  }
}

.row{
    display: flex;
    justify-content: space-between;
    width: 450px;
    margin: 5px auto;
}
</style>