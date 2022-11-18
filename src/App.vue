<template>
<div>
  <LetterRow v-for="guess in guesses" :word="guess"/>
  <LetterRow v-if="guesses.length<6" :word="currentGuess"/>
  <LetterRow v-for="index in emptyRows" :key="index" word="empty"/>
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
      return 5-this.guesses.length >= 0 ? 5-this.guesses.length : 0
    },
    isGameOver(){
      return this.guesses.length === 6 || this.guessedLetters.green.size === 5
    }
  },
  data(){
    return{
      target: 'maple',
      guessedLetters: {
        gray: new Set(),
        yellow: new Set(),
        green: new Set()
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

      let removed = []
      
      // check for exact matches
      guessArray.forEach((c,i)=>{
        console.log(`--check for exact matches ${c} == ${targetArray[i]}`)
        if(targetArray[i]===c){
          console.log(`adding ${c} to green`)
          this.guessedLetters.green.add(c)
          // "remove" the guessed letter from the target array
          removed.push(c)

          console.log(`         targetArray: ${targetArray}`)
          console.log(`         guessArray: ${guessArray}`)
          console.log(`         greenArray: ${new Array(...this.guessedLetters.green).join(' ')}`)
        }
      })

      // check for yellows
    guessArray = guessArray.filter(x => !removed.includes(x))
    targetArray = targetArray.filter(x => !removed.includes(x))

      for(let i=0; i<guessArray.length; i++){
        for(let j=0; j<targetArray.length; j++){
          if(guessArray[i] === targetArray[j]){
            console.log(`adding ${guessArray[i]} to yellow`)
            this.guessedLetters.yellow.add(guessArray[i])
            removed.push(guessArray[i])
            console.log(`         targetArray: ${targetArray}`)
            console.log(`         guessArray: ${guessArray}`)
            console.log(`         yellowArray: ${new Array(...this.guessedLetters.yellow).join(' ')}`)
          }
        }
      }

      // the rest are gray

      guessArray = guessArray.filter(x => !removed.includes(x))
      targetArray = targetArray.filter(x => !removed.includes(x))

      guessArray.forEach(letter => {
        console.log(`adding ${letter} to gray`)
        this.guessedLetters.gray.add(letter)
        console.log(`         targetArray: ${targetArray}`)
        console.log(`         guessArray: ${guessArray}`)
        console.log(`         grayArray: ${new Array(...this.guessedLetters.gray).join(' ')}`)
        })

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
