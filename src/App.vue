<template>
<Modal v-if="showModal" :target="target" :guesses="evaluatedGuesses" @closeModal="closeModal"/>
<div class="game">
  <div class="rows">
    <LetterRow v-for="guess in evaluatedGuesses" :evaluatedWord="guess" />
    <div :class="{'shake' : animated}"><LetterRow v-if="guesses.length<6" :word="currentGuess"/></div>
    <LetterRow v-for="index in emptyRows" :key="index" word=""/>
  </div>
  <div class="keyboard">
    <Keyboard v-on:keyPressed="addLetter" :colorMap="colorMap"/>
  </div>
  </div>
</template>

<script>
import Modal from "./components/Modal.vue"
import Keyboard from "./components/Keyboard.vue"
import LetterRow from "./components/LetterRow.vue"
import targets from "@/targets.js"
import validWords from "@/valid.js"
import axios from "axios"

export default {
  name: "App",
  components: {
    LetterRow,
    Keyboard,
    Modal
  },
  data(){
    return{
      target: '', // set to random word from targets.js
      colorMap: {},
      guesses: [],
      evaluatedGuesses:[],
      currentGuess:'',
      showModalToggle: false,
      animated: false,
      isBlocked: false
    }
  },
  created() {
    window.addEventListener('keydown', (e) => {
      this.addLetter(e.key)
    });
  },
  mounted(){
    this.target = targets[Math.floor(Math.random() * (targets.length))]
    // console.log("Target: "+this.target)
    // console.log(targets)
  },
  computed:{
    emptyRows(){
      return 5-this.guesses.length >= 0 ? 5-this.guesses.length : 0
    },
    isGameOver(){
      // get the last evaluated guess
      if(this.evaluatedGuesses.length>0){
        let lastGuess = this.evaluatedGuesses[this.evaluatedGuesses.length-1]
        // console.log("filtered guess: ")
        // console.log(lastGuess.filter(x=>x[1]==2))
        // console.log(`isGameOver:  ${this.guesses.length === 6 || lastGuess.filter(x=>x[1]==2).length == 5}`)

        return this.guesses.length === 6 || lastGuess.filter(x=>x[1]==2).length == 5
      }
      else{
        return false
      }
    },
    showModal(){
      return this.isGameOver && this.showModalToggle
    }
  },
  methods:{
    shakeHandler() {
      // call when an invalid word is submitted
      const self = this
      self.animated = true
      setTimeout(() => {
        self.animated = false
      }, 1000)
    },
    closeModal(){
      this.showModalToggle = false
    },
    openModal(){
      this.showModalToggle = true
    },
    printBoard(arr){
      // for debugging purposes
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
      // console.log(str)
    },
    addLetter(letter){
      // add a letter to the current guess
      let validKeys = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z','backspace','enter']
      if(this.isGameOver){
        this.openModal()
      }

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
      // console.log(this.currentGuess)
    },
    submitGuess(){
      // only one request at a time
      if(!this.isBlocked){
        // first check the list of valid words that are known to not be in the dictionary API
        if(validWords.includes(this.currentGuess)){
          // console.log("your guess is valid!!")
          this.evaluateGuess(this.currentGuess)
          this.guesses.push(this.currentGuess)
          this.currentGuess = ''
          this.isBlocked = false;
          return;
        }
        this.isBlocked = true;  // block requests until this one is completed
        // console.log('submitting guess: '+this.currentGuess)
        axios
        .get('https://api.dictionaryapi.dev/api/v2/entries/en/'+this.currentGuess)
        .then(response => {
          // console.log(response)
          this.evaluateGuess(this.currentGuess)
          this.guesses.push(this.currentGuess)
          this.currentGuess = ''
          this.isBlocked = false; // unblock requests
          })
        .catch(e => {
            // console.log(e)
            // console.log(`${this.currentGuess} is not a real word`)
            this.shakeHandler()
            this.isBlocked = false; // unblock requests
        })
      }
      else{
        console.log("blocked while processing request")
      }
    },
    evaluateGuess(guess){
      // After a guess is submitted and validated in the dictionary, evaluate it against the target
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
      // console.log(this.colorMap)

      // save the evaluated guess
      this.evaluatedGuesses.push(guessArr)

    if(this.isGameOver || this.evaluatedGuesses.length == 6){
      // wait a second before displaying the score modal
      setTimeout(this.openModal,1300)
      // console.log("You win!")
    }
    }
  }
};
</script>

<style>
:root {
  --yellow: rgb(206, 169, 6);
  --green: rgb(0, 78, 0);
  --gray: rgb(52, 52, 52);
  --empty-color: rgba(0, 0, 0, 0.504);
  --background: rgb(44, 62, 80);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
  margin-top: 60px;
  width: 100%;
}

@media (max-width: 600px) {
  #app{
    margin-top: 10px;
  }
}

*{
  background-color: var(--background)
}

.rows{
  margin: auto;
  width: 100%;
}

/* shake row when an invalid word is submitted */
.shake {
  animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
  transform: translate3d(0, 0, 0);
}
@keyframes shake {
  10%, 90% {
    transform: translate3d(-1px, 0, 0);
  }
  20%, 80% {
    transform: translate3d(2px, 0, 0);
  }
  30%, 50%, 70% {
    transform: translate3d(-4px, 0, 0);
  }
  40%, 60% {
    transform: translate3d(4px, 0, 0);
  }
}

</style>
