<template>
  <div>
    <LetterRow v-for="guess in evaluatedGuesses" :evaluatedWord="guess" />
    <LetterRow v-if="guesses.length<6" :word="currentGuess"/>
    <LetterRow v-for="index in emptyRows" :key="index" word=""/>
  </div>
  <Keyboard v-on:keyPressed="addLetter" :colorMap="colorMap"/>
</template>

<script>
import Keyboard from "./components/Keyboard.vue"
import LetterRow from "./components/LetterRow.vue"
import targets from "@/targets.js"
import axios from "axios"

export default {
  name: "App",
  components: {
    LetterRow,
    Keyboard
  },
  created() {
    window.addEventListener('keydown', (e) => {
      this.addLetter(e.key)
    });
  },
  mounted(){
    this.target = targets[Math.floor(Math.random() * (targets.length))]
    console.log("Target: "+this.target)
    console.log(targets)
  },
  computed:{
    emptyRows(){
      return 5-this.guesses.length >= 0 ? 5-this.guesses.length : 0
    },
    isGameOver(){
      return this.guesses.length === 6 || Object.entries(this.colorMap).filter(x => x[1] == 2).length == 5
    }
  },
  data(){
    return{
      target: '', // set to random word from targets.js
      colorMap: {},
      guesses: [],
      evaluatedGuesses:[],
      currentGuess:''
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
      let validKeys = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z','backspace','enter']

      if(!this.isGameOver && validKeys.includes(letter.toLowerCase())){

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
      axios
      .get('https://api.dictionaryapi.dev/api/v2/entries/en/'+this.currentGuess)
      .then(response => {
        console.log(response)
        this.evaluateGuess(this.currentGuess)
        this.guesses.push(this.currentGuess)
        this.currentGuess = ''
        })
      .catch(e => {
          console.log(e)
          console.log(`${this.currentGuess} is not a real word`)
      })
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

      
      // update keyboard colors
      // if the letter isn't already green, set it to whichever color the evaluated guess indicates
      guessArr.forEach(letter => {
        if(this.colorMap[letter[0]] != 2){
          this.colorMap[letter[0]] = letter[1]
        }
      })

      this.printBoard(guessArr)
      console.log(this.colorMap)

      // save the evaluated guess
      this.evaluatedGuesses.push(guessArr)

    if(this.isGameOver){
      console.log("You win!")
    }
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
