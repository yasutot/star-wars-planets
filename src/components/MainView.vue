<template>
  <div class="h-100">
    <div v-if="isLoading">
      <Loading/>
    </div>

    <div v-else-if="this.planets.length === 0">
      <Restart v-on:restart="restart()"/>
    </div>

    <div v-else-if="this.planet.films" class="h-100 pt-5 pb-5">
      <PlanetCard v-bind:planet="planet"
                  v-on:next-planet="pickRandomPlanet()"/>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import Loading from './Loading.vue'
  import PlanetCard from './PlanetCard.vue'
  import Restart from './Restart.vue'

  export default {
    name: 'MainView',
    data: () => {
      return {
        planets: [],
        usedPlanets: [],
        planet: Object,
        isLoading: true
      }
    },
    components: {
      Loading,
      PlanetCard,
      Restart
    },
    async mounted() {
      await this.getAllPlanets('https://swapi.co/api/planets/')
      await this.pickRandomPlanet()
      this.isLoading = false
    },
    methods: {
      getAllPlanets(url){
        let aysncPlanets = []
        return axios.get(url)
          .then(response => {
            this.planets = this.planets.concat(response.data.results);
            if (response.data.next) {
              return this.getAllPlanets(response.data.next);
            }
          })
          .catch(() => {
            return this.planets
          })
      },
      pickRandomPlanet() {
        this.planet = null;
        if (!this.planets.length) return
        let planetPosition = Math.floor(Math.random() * (this.planets.length) + 1) - 1;
        this.planet = this.planets[planetPosition]
        this.usedPlanets.push(this.planets.splice(planetPosition,1)[0])
      },
      restart() {
        this.planets = this.usedPlanets.splice(0, this.usedPlanets.length);
      }
    }
  }
</script>

<style scoped lang="scss">
</style>