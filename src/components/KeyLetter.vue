<template>
  <div class="letterkey">{{letter}}</div>
</template>

<script>
export default {
props:{
    letter: String
}
}
</script>

<style>
.letterkey{
    border: 1px solid yellow;
    border-radius: 10px;
    color: white;
    font-size: 20px;
    padding: 10px;
    min-width: 17px;
}
</style>