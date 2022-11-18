<template>
  <div class="letterkey" @click="keyPress">{{letter}}</div>
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
},
computed:{
    keyColor(){
        const colors = {
            0: 'gray',
            1: 'yellow',
            2: 'green'
        }
        return colors[this.color]
    }
}
}
</script>

<style>
.letterkey{
    border: 1px solid yellow;
    border-radius: 10px;
    color: white;
    font-size: 15px;
    padding: 10px;
    min-width: 17px;
    cursor: pointer;
}
</style>