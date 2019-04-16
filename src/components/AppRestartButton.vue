<template>
  <button class="btn btn-starwars" id="button-restart" v-on:click="$emit('restart')">RESTART</button>
</template>

<script>
  export default {
    name: 'AppRestartButton'
  }
</script>

<style scoped lang="scss">
  #button-restart {
    position: fixed;
    width: 15rem;
    height: 15rem;
    font-size: 2rem;
    top: calc( 50% - ( 15rem / 2) );
    right: calc( 50% - ( 15rem / 2) );
  }
</style>