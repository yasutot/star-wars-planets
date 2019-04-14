<template>
  <div>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'PlanetCard',
    data: () => {
      return {
        planets: []
      }
    },
    async mounted() {
      await this.getAllPlanets('https://swapi.co/api/planets/')
      console.log(this.planets)
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
      }
    }
  }
</script>

<style scoped lang="scss">

</style>