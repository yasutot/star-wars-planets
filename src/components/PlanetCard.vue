<template>
  <b-container class="h-100">
    <b-row align-v="center" class="h-100">
      <div v-if="isLoading" >
        LOADING
      </div>

      <div v-else-if="this.planets.length === 0" align-v="center">
        <b-button variant="primary" v-on:click="restart()">Restart</b-button>
      </div>

      <div v-else-if="this.planet.films" class="h-80 w-100">
        <b-row align-v="center" class="h-80">
          <b-col class="h-100">
            <div class="w-100 h-100">
              <b-card no-body class="card--planet w-100 h-100">
                <h1 slot="header" class="mb-0">{{ this.planet.name.toUpperCase() }}</h1>
                <b-card-body>
                  <b-row align-v="center" class="h-100">
                    <b-col class="h-100" align-v="center" >
                      <h2>Population:  {{ this.planet.population }}</h2>
                      <h2>Climate: {{ this.planet.climate }}</h2>
                      <h2>Terrain: {{ this.planet.terrain }}</h2>
                    </b-col>
                  </b-row>
                </b-card-body>
                <b-card-footer>
                  Featured in {{ this.planet.films.length}} {{ this.planet.films.length | pluralizeFilms }}
                </b-card-footer>
              </b-card>
            </div>
          </b-col>
        </b-row>

        <b-row class="pt-2 h-20" align-v="center">
          <b-button variant="primary" class="mx-auto p-2" size="lg" v-on:click="pickRandomPlanet()">Next</b-button>
        </b-row>
      </div>
    </b-row>
  </b-container>
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
  .h-80 {
    height: 80%;
  }
  .h-20 {
    height: 20%;
  }
</style>