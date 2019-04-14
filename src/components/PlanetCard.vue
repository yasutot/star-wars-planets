<template>
  <div>
    <div v-if="isLoading">
      LOADING
    </div>
    <div v-else-if="this.planets.length === 0">
      <b-button variant="primary" v-on:click="restart()">Restart</b-button>
      RESTART
    </div>
    <div v-else-if="this.planet.films">
      <b-card :header="this.planet.name">
        <b-card-text>Population:  {{ this.planet.population }}</b-card-text>
        <b-card-text>Climate: {{ this.planet.climate }}</b-card-text>
        <b-card-text>Terrain: {{ this.planet.terrain }}</b-card-text>
        <b-card-text>Featured in {{ this.planet.films.length}} {{ this.planet.films.length | pluralizeFilms }}</b-card-text>
      </b-card>
      <b-button variant="primary" v-on:click="pickRandomPlanet()">Next</b-button>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'PlanetCard',
    data: () => {
      return {
        planets: [],
        usedPlanets: [],
        planet: Object,
        isLoading: true
      }
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
    },
    filters: {
      pluralizeFilms: (val => {
        if (val === 1) return "film"
        return "films"
      })
    }
  }
</script>

<style scoped lang="scss">

</style>