<template>
<div>
  
  <!-- <LetterRow :word="this.guesses[0]?this.guesses[0]:''"/>
  <LetterRow :word="this.guesses[1]?this.guesses[1]:''"/>
  <LetterRow :word="this.guesses[2]?this.guesses[2]:''"/>
  <LetterRow :word="this.guesses[3]?this.guesses[3]:''"/>
  <LetterRow :word="this.currentGuess"/> -->

  <LetterRow v-for="guess in guesses" :word="guess"/>
  <LetterRow v-if="guesses.length < 5" :word="currentGuess"/>
  <LetterRow v-if="!isGameOver" v-for="index in emptyRows" :key="index" word=""/>
</div>
<Keyboard v-on:keyPressed="addLetter"/>
</template>

<script>
import Keyboard from "./components/Keyboard.vue";
import LetterRow from "./components/LetterRow.vue";
export default {
  name: "App",
  components: {
    LetterRow,
    Keyboard
  },
  computed:{
    emptyRows(){
      return 4-this.guesses.length >= 0 ? 4-this.guesses.length : 0
    },
    isGameOver(){
      return this.guesses.length === 5
    }
  },
  data(){
    return{
      guessedLetters: {
        gray: [],
        yellow: [],
        green: []
      },
      guesses: ['raise','earth','maple'],
      currentGuess:'',
    }
  },
  methods:{
    addLetter(letter){
      if(!this.isGameOver){

        if(letter === 'backspace' && this.currentGuess.length > 0){
          this.currentGuess = this.currentGuess.substring(0,this.currentGuess.length-1)
      }
      else if(letter !== 'enter' && letter !== 'backspace' && this.currentGuess.length < 5){
        this.currentGuess+=letter
      }
      else if(letter === 'enter' && this.currentGuess.length == 5){
        this.submitGuess()
      }
        }
      console.log(this.currentGuess)
    },
    submitGuess(){
      console.log('submitting guess: '+this.currentGuess)
      this.guesses.push(this.currentGuess)
      this.currentGuess = ''
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
*{
  background-color: #2c3e50;
}
</style>
