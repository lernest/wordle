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
    border: solid black 1px;
    display: flex;
    justify-content: space-between;
    margin: auto;
    padding: 10px;
    font-size: 50px;

}
</style>