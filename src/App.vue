<template>
  <LetterRow word="Hello" />
</template>

<script>
import LetterRow from "./components/LetterRow.vue";

export default {
  name: "App",
  components: {
    LetterRow,
  },
  data(){
    return{
      guesses: {
        gray: [],
        yellow: [],
        green: []
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
</style>
