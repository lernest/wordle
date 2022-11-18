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
                return 'gray'
            }
            if(this.color == 1){
                return 'yellow'
            }
            if(this.color == 2){
                return 'green'
            }
            else{
                return 'black'
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
.black{
    background-color:black
}
.gray{
    background-color: gray;
    color: red
}
.yellow{
    background-color: yellow
}
.green{
    background-color: green
}
</style>