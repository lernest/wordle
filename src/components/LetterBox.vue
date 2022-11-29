<template>
  <div class="letter" :class="colorClass">{{letter.toUpperCase()}}</div>
</template>

<script>
export default {
    props:{
        letter: String,
        color: Number
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
@media (max-width: 600px) {
  .letter{
    margin: 10px;
    flex-basis: 12%;
    height: 50px;
  }
}

.letter{
    margin: auto;
    padding: 10px;
    font-size: 45px;
    width: 60px;
    height: 60px;
    color: white;
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