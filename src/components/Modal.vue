<template>

<div id="myModal" class="modal">

    <!-- Modal content -->
    <div class="modal-content">
    <span class="close" @click="handleClick">&times;</span>
    <p>The word was: <span class="target">{{target.toUpperCase()}}</span></p>
    <div class="score">{{isWinner ? guesses.length : 'X'}}/6</div>
    <div class="scoreboard">
        <p v-for="row in scoreboard" v-html="row"></p>
    </div>
    <!-- <button @click="copy">click</button> -->
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
    //     copy() {
    //         navigator.clipboard.writeText()
    //     },
    //     copyURL() {
    //   var Url = this.$refs.mylink;
    //   Url.innerHTML = window.location.href;
    //   console.log(Url.innerHTML)
    //   Url.select();
    //   document.execCommand("copy");
    // },
        handleClick(){
            console.log("clicked")
            this.$emit('closeModal')
        },
        getSymbol(pair){
            console.log("       getting symbol for pair: "+pair)
            console.log(this.currentTheme)
            let symbol
            switch(pair[1]){
                case 0:
                    // console.log(0)
                    symbol= this.themes[this.currentTheme][0]
                    break;                
                case 1:
                    // console.log(1)
                    symbol= this.themes[this.currentTheme][1]
                    break;
                case 2:
                    // console.log(2)
                    symbol= this.themes[this.currentTheme][2]
                    break;            
                }
                console.log("symbol: "+symbol)
                return symbol;
        },
        getScoreboardRow(guessArray){
            console.log("getting scoreboard row")
            console.log("   guessArray:  "+guessArray)
            let result =  guessArray.map(x=>`<span> ${this.getSymbol(x)} </span>`)
            console.log("   result: "+result)
            console.log("   string result: "+result)
           
            return result.join(" ")
        }
    },
    computed:{
        markup(){
            return "<span> &#x231A </span>"
        },
        isWinner(){
            return this.guesses[this.guesses.length-1].map(x=>x[0]).join('')==this.target.toUpperCase()
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
            inputText:"&#128159",
            // themes:{
            //     hearts:[`&#128420`,'&#128155','&#128154'],
            //     blocks:['&#','&#','&#'],
            //     faces:['&#128542','&#128559','&#128526'],
            //     books:['&#128211','&#128210','&#128215'],
            //     stars:[]
            // },
            themes:[['&#128546','&#128559','&#128526','faces'],[`&#128420`,'&#128155','&#128154','hearts'],['_','..','$'],[' ',' ',' ']],
            currentTheme: 1
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
  background-color: rgb(0,0,0);  /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
  /* text-align: center; */
}

.modal h2,h3,p{
    background-color: rgba(0,0,0,0);
    text-align: center;
}


/* Modal Content/Box */
.modal-content {
  background-color:  rgba(0,0,0,0.7);
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
  background-color: rgba(0,0,0,0);
}

.close:hover,
.close:focus {
    color: rgb(128, 128, 128);
  text-decoration: none;
  cursor: pointer;
}

.target{
    font-weight: bold;
    font-size: 1.2em;
    background-color: rgba(0,0,0,0);
}
.scoreboard p{
    margin: 1px;
}
.scoreboard{
    background-color: rgba(44, 62, 80,0.7);
    padding: 5px;
    width: 9em;
    margin: auto;
}
.score{
    font-size:1.4em;
    text-align: center;
    background-color: rgba(0,0,0,0);
    margin-bottom: 20px
}
</style>