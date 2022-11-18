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
  created() {
    window.addEventListener('keydown', (e) => {
      console.log(e.key)
      this.addLetter(e.key)
    });
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
      colorMap: {},
      guesses: [],//['raise','earth','maple'],
      evaluatedGuesses:[],
      currentGuess:'',
    }
  },
  methods:{
    printBoard(arr){
      let str = ''
      arr.forEach(x => {
          if(x[1] == 1){
          // yellow
              str += `| ( ${x[0]} ) |`
          }
          else if(x[1] == 2){
          // green
              str += `| * ${x[0]} * |`
          }
          else{
          // gray
          str += `|   ${x[0]}   |`
          }
      })
      console.log(str)
    },
    addLetter(letter){
      if(!this.isGameOver){

        if(letter.toLowerCase() === 'backspace' && this.currentGuess.length > 0){
            this.currentGuess = this.currentGuess.substring(0,this.currentGuess.length-1)
        }
        else if(letter.toLowerCase() !== 'enter' && letter.toLowerCase() !== 'backspace' && this.currentGuess.length < 5){
          this.currentGuess+=letter
        }
        else if(letter.toLowerCase() === 'enter' && this.currentGuess.length == 5){
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
      /* 
      'guessArr' is an array of tuples containing each character in the current guess and the evaluated color
            ex: [['a',2],['m',1],['l',2],['e',0]]
            color key:
              0 -- gray
              1 -- yellow
              2 -- green
      */
      let guessArr = guess.toUpperCase().split('').map(x => [x,0])
      let targetArr = this.target.toUpperCase().split('')
      
      for(let i=0; i<guessArr.length; i++){
          // if each letter is in target array -> flip to yellow (1)
              if(targetArr.includes(guessArr[i][0])){
                  guessArr[i][1] = 1
              }
      
          // if the letter matches the same index -> flip to green (2)
              if(guessArr[i][0] == targetArr[i]){
                  guessArr[i][1] = 2
              }
      
          // otherwise keep it gray
      }

      /*
        update keyboard colors
        if the letter isn't already green, set it to whichever color the evaluated guess indicates
      */
      guessArr.forEach(letter => {
        if(this.colorMap[letter[0]] != 2){
          this.colorMap[letter[0]] = letter[1]
        }
      })

      this.printBoard(guessArr)
      console.log(this.colorMap)

      // save the evaluated guess
      this.evaluatedGuesses.push(guessArr)

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
