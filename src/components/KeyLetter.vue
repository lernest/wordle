<template>
  <div class="letterkey" :class="colorClass" @click="keyPress">{{letter}}</div>
</template>

<script>
export default {
methods:{
    keyPress(e){
        this.$emit('keyPress')
    }
},
props:{
    letter: String,
    color: Number
},
mounted(){
    console.log(`mounted key letter: ${this.letter}  color: ${this.color}`)
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
.letterkey{
    border: 1px solid rgb(18, 18, 18);
    background-color: rgb(82, 82, 82);
    border-radius: 10px;
    color: white;
    font-size: 15px;
    padding: 10px;
    min-width: 17px;
    cursor: pointer;
}
.black{
    background-color:black;
}
.gray{
    background-color: rgb(52, 52, 52);
}
.yellow{
    background-color: rgb(206, 169, 6);
}
.green{
    background-color: rgb(0, 78, 0);
}
</style>