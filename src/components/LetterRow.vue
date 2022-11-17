<template>
<div class="row">
  <LetterBox :letter="word[0]"/>
  <LetterBox :letter="word[1]"/>
  <LetterBox :letter="word[2]"/>
  <LetterBox :letter="word[3]"/>
  <LetterBox :letter="word[4]"/>
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
    border: 1px solid red;
    width: 500px;
    margin: auto;
}
</style>