<template>
  <div class="card w-100 h-100 shadow-sm">
    <div class="card-header bg-white">
      <h1 class="mb-0">{{ planet.name.toUpperCase() }}</h1>
    </div>
    <div class="card-body pt-5 pb-5">
      <span class="w-100">POPULATION: {{ planet.population.toUpperCase() }}</span>
      <span class="w-100">CLIMATE: {{ planet.climate.toUpperCase() }}</span>
      <span class="w-100">TERRAIN: {{ planet.terrain.toUpperCase() }}</span>
      <span class="w-100 mt-2 justify-content-center">FEATURED IN {{ planet.films.length }} {{ planet.films.length | pluralizeFilms }}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'AppPlanetCard',
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
  @media only screen and (max-width: 320px) {
    .card-header h1 {
      font-size: 1.8rem
    }
    .card-body span {
      font-size: 1rem !important;
    }
    // .card-body > span:last-child {
    //   font-size: 1rem;
    //   padding-top: 1rem;
    // }
  }
</style>