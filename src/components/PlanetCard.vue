<template>
  <div class="container h-100">
    <div v-if="isLoading">
      <Loading/>
    </div>

    <div v-else-if="this.planets.length === 0" align-v="center">
      <button class="btn" id="button-restart" variant="primary" v-on:click="restart()">RESTART</button>
    </div>

    <div v-else-if="planet.films" class="h-100 pt-5 pb-5">
      <div class="row h-90 pb-4">
        <div class="col">
          <div class="card w-100 h-100 shadow-sm">
            <div class="card-header bg-white">
              <h1 class="mb-0">{{ this.planet.name.toUpperCase() }}</h1>
            </div>
            <div class="card-body pt-5 pb-5">
              <span class="w-100">POPULATION: {{ this.planet.population.toUpperCase() }}</span>
              <span class="w-100">CLIMATE: {{ this.planet.climate.toUpperCase() }}</span>
              <span class="w-100">TERRAIN: {{ this.planet.terrain.toUpperCase() }}</span>
              <span class="w-100 mt-2 justify-content-center">FEATURED IN {{ this.planet.films.length }} {{ this.planet.films.length | pluralizeFilms }}</span>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <button class="btn mx-auto shadow" v-on:click="pickRandomPlanet()">NEXT</button>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import Loading from './Loading.vue'

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
    components: {
      Loading
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
        if (val === 1) return "FILM"
        return "FILMS"
      })
    }
  }
</script>

<style scoped lang="scss">
  @font-face{
    font-family:STARWARS;
    font-style:normal;
    font-weight:400;
    src:url('../assets/fonts/STARWARS.woff') format('woff'),
    url('../assets/fonts/STARWARS.ttf') format('truetype')
  }

  .h-90 {
    height: 90%;
  }
  .h-20 {
    height: 20%;
  }
  .card-body {
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    > span {
      font-size: 1.5rem;
      display: flex;
      text-align: left;
    }

    > span:last-child {
      font-size: 1.3rem;
      padding-top: 1rem;
    }
  }
  .card-header {
    font-family: STARWARS;
  }
  button {
    color: #ac856c;
    border: 2px solid #ac856c;
    background: transparent;
    width: 10rem;
    font-family: STARWARS;
  }

  button:active {
    background: #ac856c;
    color: #212529;
  }
  #button-restart {
    position: fixed;
    width: 15rem;
    height: 15rem;
    font-size: 2rem;
    top: calc( 50% - ( 15rem / 2) );
    right: calc( 50% - ( 15rem / 2) );
  }
  @media only screen and (max-width: 768px) {
    .card-body > span {
      font-size: 1.3rem;
    }
  }

</style>