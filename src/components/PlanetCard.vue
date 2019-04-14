<template>
  <div>
    <b-card :header="this.planet.name" v-if="this.planet.films">
      <b-card-text>Population:  {{ this.planet.population }}</b-card-text>
      <b-card-text>Climate: {{ this.planet.climate }}</b-card-text>
      <b-card-text>Terrain: {{ this.planet.terrain }}</b-card-text>
      <b-card-text>Featured in {{ this.planet.films.length}} {{ this.planet.films.length | pluralizeFilms }}</b-card-text>
    </b-card>
    <b-button variant="primary">Next</b-button>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'PlanetCard',
    data: () => {
      return {
        planets: [],
        planet: Object
      }
    },
    async mounted() {
      await this.getAllPlanets('https://swapi.co/api/planets/')
      await this.pickRandomPlanet()
      console.log(this.planet)
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
        let planetPosition = Math.floor(Math.random() * (61) + 1) - 1;
        this.planet = this.planets[planetPosition]
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