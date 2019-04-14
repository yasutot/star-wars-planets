<template>
  <div id="app" class="container">
    <PlanetCard msg="Welcome to Your Vue.js App"/>
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
// #app {
//   font-family: 'Avenir', Helvetica, Arial, sans-serif;
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   text-align: center;
//   color: #2c3e50;
//   margin-top: 60px;
// }
#app {
  background-color: "eaeaea"
}
</style>
