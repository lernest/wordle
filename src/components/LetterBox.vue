<template>
  <div class="letter" :class="colorClass">{{letter.toUpperCase()}}</div>
</template>

<script>
export default {
    props:{
        letter: String,
        color: Number
    },
    mounted(){
        console.log(`letter: ${this.letter} color: ${this.color} class: ${this.colorClass}`)
    },
    computed:{
        colorClass(){
            if(this.color == 0){
                return 'grayLetter'
            }
            if(this.color == 1){
                return 'yellowLetter'
            }
            if(this.color == 2){
                return 'greenLetter'
            }
            else{
                return 'blackLetter'
            }
        }
    }
}
</script>

<style>
.letter{
    border: solid rgb(121, 234, 170) 1px;
    margin: auto;
    padding: 10px;
    font-size: 50px;
    width: 70px;
    height: 70px;
    color: white;
}
.blackLetter{
    background-color:black;
}
.grayLetter{
    background-color: rgb(52, 52, 52);
}
.yellowLetter{
    background-color: rgb(206, 169, 6);
}
.greenLetter{
    background-color: rgb(0, 78, 0);
}
</style>