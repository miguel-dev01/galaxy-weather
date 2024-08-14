<!-- src/components/Weather.vue -->
<template>
    <div class="weather-container">
      <h2>Datos Meteorológicos</h2>
      <div v-if="weather">
        <p><strong>Localidad:</strong> {{ weather.name }}</p>
        <p><strong>Estado Meteorológico:</strong> {{ weather.weather[0].description }}</p>
        <p><strong>Temperatura Actual:</strong> {{ weather.main.temp }}°C</p>
        <p><strong>Temperatura Máxima:</strong> {{ weather.main.temp_max }}°C</p>
        <p><strong>Temperatura Mínima:</strong> {{ weather.main.temp_min }}°C</p>
        <p><strong>Presión Atmosférica:</strong> {{ weather.main.pressure }} hPa</p>
        <p><strong>Humedad Relativa:</strong> {{ weather.main.humidity }}%</p>
        <p><strong>Visibilidad:</strong> {{ visibility }} metros</p>
        <p><strong>Información del Viento:</strong> {{ weather.wind.speed }} m/s, {{ windDirection }}°</p>
      </div>
      <div v-else>
        <p>Cargando...</p>
      </div>
    </div>
</template>
  
<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        weather: null,
        visibility: null,
        windDirection: null,
        city: 'London',
        apiKey: '093896d914c405d7d9a795a1fc0b2984',
      };
    },
    mounted() {
      this.fetchWeather();
    },
    methods: {
      fetchWeather() {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.apiKey}`;
        axios.get(url)
          .then(response => {
            this.weather = response.data;
            this.visibility = response.data.visibility; 
            this.windDirection = response.data.wind.deg;
          })
          .catch(error => {
            console.error('Error al obtener los datos del clima', error);
          });
      }
    }
  };
</script>
  
<style scoped>
  .weather-container {
    background-color: #f0f0f0;
    border-radius: 10px;
    padding: 20px;
    width: 300px;
    margin: 0 auto; /* Centra el contenedor horizontalmente */
    text-align: center;
  }
  
  h2 {
    margin-bottom: 20px;
  }
  
  p {
    margin: 5px 0;
    font-size: 16px;
  }
</style>
  