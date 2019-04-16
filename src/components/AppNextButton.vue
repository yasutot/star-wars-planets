<template>
  <button class="btn mx-auto btn-starwars" v-on:click="$emit('next')">NEXT</button>
</template>

<script>
  export default {
    name: 'AppNextButton'
  }
</script>