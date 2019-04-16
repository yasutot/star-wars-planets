<template>
  <div class="container h-100">
    <div v-if="isLoading">
      <AppLoadingIcon/>
    </div>

    <div v-else-if="this.planets.length === 0">
      <AppRestartButton v-on:restart="restart()"/>
    </div>

    <div v-else-if="this.planet.films" class="h-100 pt-5 pb-5">
      <div class="row h-90 pb-4">
        <div class="col h-100">
          <AppPlanetCard v-bind:planet="planet"/>
        </div>
      </div>
      <div class="row h-10">
        <AppNextButton  v-on:next="pickRandomPlanet()"/>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import AppLoadingIcon from './AppLoadingIcon.vue'
  import AppRestartButton from './AppRestartButton.vue'
  import AppNextButton from './AppNextButton.vue'
  import AppPlanetCard from './AppPlanetCard.vue'

  export default {
    name: 'MainContainer',
    data: () => {
      return {
        planets: [],
        usedPlanets: [],
        planet: Object,
        isLoading: true
      }
    },  
    components: {
      AppLoadingIcon,
      AppRestartButton,
      AppNextButton,
      AppPlanetCard
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