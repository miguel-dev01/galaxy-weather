<template>
  <GalaxyWeather />
</template>

<script>
  import GalaxyWeather from './components/GalaxyWeather.vue'

  export default {
    name: 'App',
    components: {
      GalaxyWeather 
    }
  }
</script>

<style>
  #app {
    font-family: 'Poppins', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
