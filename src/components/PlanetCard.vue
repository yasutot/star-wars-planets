<template>
  <div class="container h-100">
    <div class="row h-90 pb-4">
      <div class="col h-100">
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
    <div class="row h-10">
      <button class="btn mx-auto btn-starwars" v-on:click="$emit('next-planet')">NEXT</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'PlanetCard',
    props: ['planet'],
    filters: {
      pluralizeFilms: (val => {
        if (val === 1) return "FILM"
        return "FILMS"
      })
    }
  }
</script>

<style scoped lang="scss">
  .card-header {
    font-family: STARWARS;
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
  @media only screen and (max-width: 768px) {
    .card-body > span {
      font-size: 1.3rem;
    }
  }
</style>