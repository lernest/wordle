<template>
  <div class="letter">{{letter.toUpperCase()}}</div>
</template>

<script>
export default {
    props:{
        letter: String
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
</style>