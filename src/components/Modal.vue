<template>

<div id="myModal" class="modal">

    <!-- Modal content -->
    <div class="modal-content">
    <span class="close" @click="handleClick">&times;</span>
    <h2>Score</h2>
    <p>The word was: <span class="target">{{target.toUpperCase()}}</span></p>
    <p>&#128159</p>
    <p>{{guesses}}</p>
    <p>{{guesses[guesses.length-1].map(x=>x[0]).join('')==target.toUpperCase()}}</p>
        <p>{{scoreboard}}</p>
    </div>

</div>

</template>

<script>
export default {
    props:{
        target: String,
        guesses: Array
    },
    methods:{
        handleClick(){
            console.log("clicked")
            this.$emit('closeModal')
        },
        getSymbol(pair){
            console.log("       getting symbol for pair: "+pair)
            let symbol
            switch(pair[1]){
                case 0:
                    // console.log(0)
                    symbol= "_"
                    break;                
                case 1:
                    // console.log(1)
                    symbol= ".."
                    break;
                case 2:
                    // console.log(2)
                    symbol= "X"
                    break;            
                }
                console.log("symbol: "+symbol)
                return symbol;
        },
        getScoreboardRow(guessArray){
            console.log("getting scoreboard row")
            console.log("   guessArray:  "+guessArray)
            let result =  guessArray.map(x=>this.getSymbol(x))
            console.log("   result: "+result)
           
            return guessArray.join("")
        }
    },
    computed:{
        isWinner(){
            return guesses[guesses.length-1].map(x=>x[0]).join('')==target.toUpperCase()
        },

        scoreboard(){
            console.log("scoreboard guesses"+this.guesses)
            let result = this.guesses.map(x=>this.getScoreboardRow(x))
            console.log(result)
            return result
        }
    },
    data(){
        return{
            themes:{
                hearts:{
                    0: "_",
                    1: " ",
                    2: "*"
                }
                // },
                // blocks:{

                // },
                // faces:{

                // }
            }
        }
    }
}
</script>

<style>
/* The Modal (background) */
.modal {
  /* display: none; */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 60%; /* Could be more or less, depending on screen size */
  color: rgb(255, 255, 255)
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.target{
    font-weight: bold;
    font-size: 1.2em
}
</style>