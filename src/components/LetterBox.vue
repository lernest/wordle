<template>
  <div class="letter" :style="style">{{letter.toUpperCase()}}</div>
</template>

<script>
export default {
    props:{
        letter: String,
        color: Number,
        index: Number
    },
    computed:{
        indexInt(){
            return parseInt(this.index)
        },
        style() {
            if(this.color == -1){
                return `background-color:var(--empty-color);`
            }
            return `animation: flip-${this.colorClass} 1s ${this.index*0.12}s forwards`
        },
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
@keyframes flip-yellow{
    0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    background-color:var(--empty-color)
    }
    
    100% {
    transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    background-color: var(--yellow);
    } 
}
@keyframes flip-green{
    0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    background-color:var(--empty-color)
    }
    
    100% {
    transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    background-color: var(--green);
    } 
}
@keyframes flip-gray{
    0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    background-color:var(--empty-color)
    }
    
    100% {
    transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-360deg) rotateY(0deg);
    background-color: var(--gray);
    } 
}

@media (max-width: 600px) {
  .letter{
    margin: 10px;
    flex-basis: 12%;
    height: 40px;
  }
}

.letter{
    margin: auto;
    padding: 10px;
    font-size: 45px;
    width: 60px;
    height: 60px;
    color: white;
    border: 2px solid black;
}
</style>