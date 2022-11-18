<template>
<div class="keyboard">

  <div class="keyboard-row">
    <KeyLetter v-for="letter in firstRow" :letter="letter" v-on:keyPress="addLetter(letter)" :color="colorMap[letter.toUpperCase()]"/>
  </div>

  <div class="keyboard-row">
    <KeyLetter v-for="letter in secondRow" :letter="letter" v-on:keyPress="addLetter(letter)" :color="colorMap[letter.toUpperCase()]"/>
  </div>
  
  <div class="keyboard-row">
    <KeyLetter v-for="letter in thirdRow" :letter="letter" v-on:keyPress="addLetter(letter)" :color="colorMap[letter.toUpperCase()]"/>
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
props:{
    colorMap: Object,
},
components:{
    KeyLetter
},
methods:{
    addLetter(letter){
        //console.log(letter)
        this.$emit('keyPressed', letter)
    }
}
}
</script>

<style>
.keyboard{
    width: 50%;
    margin: auto;
}
.keyboard-row{
    display: flex;
    justify-content: space-between;
}
</style>