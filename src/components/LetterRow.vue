<template>
<div v-if="!isEvaluated" class="row">
  <LetterBox :letter="this.word[0]?this.word[0]:' '"/>
  <LetterBox :letter="this.word[1]?this.word[1]:' '"/>
  <LetterBox :letter="this.word[2]?this.word[2]:' '"/>
  <LetterBox :letter="this.word[3]?this.word[3]:' '"/>
  <LetterBox :letter="this.word[4]?this.word[4]:' '"/>
</div>
<div v-if="isEvaluated" class="row">
  <LetterBox :letter="this.evaluatedWord[0][0]" :color="this.evaluatedWord[0][1]"/>
  <LetterBox :letter="this.evaluatedWord[1][0]" :color="this.evaluatedWord[1][1]"/>
  <LetterBox :letter="this.evaluatedWord[2][0]" :color="this.evaluatedWord[2][1]"/>
  <LetterBox :letter="this.evaluatedWord[3][0]" :color="this.evaluatedWord[3][1]"/>
  <LetterBox :letter="this.evaluatedWord[4][0]" :color="this.evaluatedWord[4][1]"/>
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
.row{
    display: flex;
    justify-content: space-between;
    border: 1px solid rgb(121, 234, 170);
    width: 500px;
    margin: 5px auto;
}
</style>