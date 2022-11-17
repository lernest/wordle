<template>
<div class="row">
  <LetterBox :letter="this.word[0]?this.word[0]:' '"/>
  <LetterBox :letter="this.word[1]?this.word[1]:' '"/>
  <LetterBox :letter="this.word[2]?this.word[2]:' '"/>
  <LetterBox :letter="this.word[3]?this.word[3]:' '"/>
  <LetterBox :letter="this.word[4]?this.word[4]:' '"/>
</div>
</template>

<script>
import LetterBox from './LetterBox.vue'
export default {
    props:{
        word: String
    },
    components:{
        LetterBox
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