<template>
<div>
  
  <LetterRow :word="this.guesses[0]?this.guesses[0]:'     '"/>
  <LetterRow :word="this.guesses[1]?this.guesses[1]:'     '"/>
  <LetterRow :word="this.guesses[2]?this.guesses[2]:'     '"/>
  <LetterRow :word="this.guesses[3]?this.guesses[3]:'     '"/>
  <LetterRow :word="this.guesses[4]?this.guesses[4]:'     '"/>
</div>
<Keyboard/>
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
    guess1(){
      return this.guesses[0]?this.guesses[0]:''
    }
  },
  data(){
    return{
      guessedLetters: {
        gray: [],
        yellow: [],
        green: []
      },
      guesses: ['raise','earth','maple']
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
