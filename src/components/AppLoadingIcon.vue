<template>
  <div class="spinner-border text-primary"  id="loading-spinner" role="status">
    <span class="sr-only" >Loading...</span>
  </div>
</template>

<script>
  export default {
    name: 'AppLoadingIcon'
  }
</script>

<style scoped lang="scss">
  #loading-spinner {
    display: block;
    position: fixed;
    z-index: 1031;
    top: calc( 50% - ( 3rem / 2) );
    right: calc( 50% - ( 3rem / 2) );
    color: #ac856c !important;
    width: 3rem;
    height: 3rem;
  }
</style>