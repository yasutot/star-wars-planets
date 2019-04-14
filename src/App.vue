<template>
  <div id="app">
    <PlanetCard/>
  </div>
</template>

<script>
import PlanetCard from './components/PlanetCard.vue'

export default {
  name: 'app',
  components: {
    PlanetCard
  }
}
</script>

<style lang="scss">
  @import '~bootstrap/scss/bootstrap.scss';
  @import '~bootstrap-vue/src/index.scss';

  // $body-bg: #eaeaea;
  #app {
    // font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100vh;
    background-color: #eaeaea;
  }
</style>
