<template>
<div class="keyboard">
  <div class="keyboard-row">
    <KeyLetter v-for="letter in firstRow" :letter="letter"/>
  </div>
  <div class="keyboard-row">
    <KeyLetter v-for="letter in secondRow" :letter="letter"/>
  </div>
  <div class="keyboard-row">
    <KeyLetter v-for="letter in thirdRow" :letter="letter"/>
  </div>
</div>
</template>

<script>
import KeyLetter from './KeyLetter'
export default {
data(){
    return{
        firstRow: ['q','w','e','r','t','y','u','i','o','p'],
        secondRow: ['a','s','d','f','g','h','j','k','l'],
        thirdRow: ['enter','z','x','c','v','b','n','m','backspace']
    }
},
components:{
    KeyLetter
}
}
</script>

<style>
.keyboard{
    width: 30%;
    margin: auto;
}
.keyboard-row{
    background-color: green;
    display: flex;
    justify-content: space-between;
}
</style>