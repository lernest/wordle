<template>
<div>
    <LetterRow v-for="guess in guesses" :word="guess"/>
  <LetterRow v-if="!isGameOver" :word="currentGuess"/>
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
      target: 'maple',
      guessedLetters: {
        gray: [],
        yellow: [],
        green: []
      },
      guesses: [],//['raise','earth','maple'],
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
      this.evaluateGuess(this.currentGuess)
      this.guesses.push(this.currentGuess)
      this.currentGuess = ''
    },
    evaluateGuess(guess){
      let guessArray = guess.split('')
      let targetArray = this.target.split('')
      
      // check for exact matches
      guessArray.forEach((c,i)=>{
        if(targetArray[i]===c){
          this.guessedLetters.green.push(c)
          // remove the guessed letter from the target array
          targetArray.splice(i,1)
          guessArray.splice(i,1)
        }
      })

      // check for yellows
      for(let i=0; i<guessArray.length; i++){
        for(let j=0; j<targetArray.length; j++){
          if(guessArray[i] === targetArray[j]){
            this.guessedLetters.yellow.push(guessArray[i])
            targetArray.splice(i,1)
            guessArray.splice(i,1)
          }
        }
      }

      // the rest are gray
      guessArray.forEach(letter => this.guessedLetters.gray.push(letter))

      console.log(this.guessedLetters)

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
