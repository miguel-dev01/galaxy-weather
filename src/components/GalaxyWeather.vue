<template>
  <h1>Galaxy Weather App</h1>
  <div class="weather-container">
    <div class="weather-data">
      <h2>Datos Meteorológicos</h2>

      <!-- Campo de entrada para la localidad -->
      <div class="location-input">
        <input v-model="city" type="text" placeholder="Ingrese una localidad" @keyup.enter="fetchWeather" />
        <button @click="fetchWeather">Consultar</button>
      </div>
      <br>

      <!-- Si weather tiene valor se muestran los valores, sino v-else... -->
      <div v-if="weather">
        <img :src="iconUrl" :alt="weather.weather[0].description" class="weather-icon">
        <p><strong>Localidad:</strong> {{ weather.name }}</p>
        <p><strong>Estado meteorológico:</strong> {{ weather.weather[0].description }}</p>
        <p><strong>Temperatura actual:</strong> {{ weather.main.temp }}°C</p>
        <p><strong>Temperatura máxima:</strong> {{ weather.main.temp_max }}°C</p>
        <p><strong>Temperatura mínima:</strong> {{ weather.main.temp_min }}°C</p>
        <p><strong>Presión atmosférica:</strong> {{ weather.main.pressure }} hPa</p>
        <p><strong>Humedad relativa:</strong> {{ weather.main.humidity }}%</p>
        <p><strong>Visibilidad:</strong> {{ visibility }} metros</p>
        <p><strong>Información del viento:</strong> {{ weather.wind.speed }} m/s, {{ windDirection }}°</p>
      </div>
      <div v-else>
        <p>Aún no hay datos cargados</p>
      </div>
    </div>

    <div class="weather-history">
      <h2>Historial de consultas</h2>
        <p v-for="city in getCitiesFromLocalStorage()" :key="city" @click="selectedCity(city)">
          <a href="#">{{ city }}</a>
        </p>
      <button @click="clearHistory" >Limpiar historial</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

const STORAGE_KEY = 'cities';

export default {
  data() {
    return {
      weather: null,
      visibility: null,
      windDirection: null,
      city: null,
      apiKey: '093896d914c405d7d9a795a1fc0b2984'
    };
  },
  computed: {
    iconUrl() {
      if (this.weather) {
        const iconCode = this.weather.weather[0].icon;
        return `http://openweathermap.org/img/wn/${iconCode}@2x.png`;
      }
      return '';
    },
  },
  methods: {
    fetchWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.apiKey}`;
      axios.get(url)
        .then(response => {
          this.weather = response.data;
          this.visibility = response.data.visibility;
          this.windDirection = response.data.wind.deg;
          
          this.saveCityToLocalStorage(this.city);
        })
        .catch(error => {
          console.error('Error al obtener los datos del clima', error);
        });
    },
    saveCityToLocalStorage(city) {
      // Si no hay ciudades guardadas en localStorage, se crea un array vacío
      let cities = JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
      if (!cities.includes(city)) {
        cities.push(city);
        localStorage.setItem(STORAGE_KEY, JSON.stringify(cities));
      }
    },
    getCitiesFromLocalStorage() {
      return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
    },
    // Metodo para seleccionar una ciudad del historial
    selectedCity(city) {
      this.city = city;
      this.fetchWeather();
    },
    // Limpiar el historial de consultas
    clearHistory() {
      localStorage.removeItem(STORAGE_KEY);
      location.reload(); // Recargamos la página para que se actualice el historial
    }
  }
};
</script>

<style>

body {
  margin: 0;
  padding: 0;
  background: url('../assets/white-clouds-blue-sky.jpg') no-repeat center center fixed;
  background-size: cover;
}

.weather-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 20px;
  max-width: 100%;
  margin: 0 auto;
  padding: 20px;
  flex-wrap: wrap;
}

.weather-data, .weather-history {
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 20px;
  width: 100%;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.weather-data {
  flex: 2;
}

.weather-history {
  flex: 1;
}

.location-input {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.location-input input {
  padding: 10px;
  font-size: 16px;
  flex: 1;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.location-input button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  flex-shrink: 0;
}

.location-input button:hover {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #003d7e;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  flex-shrink: 0;
}

.weather-icon {
  width: 80px;
  height: 80px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.8);
  margin-bottom: 15px;
}

h2 {
  margin-bottom: 20px;
  font-size: 1.5rem;
}

p {
  margin: 5px 0;
  font-size: 1rem;
}

.weather-history a {
  color: #007bff;
  text-decoration: underline;
  cursor: pointer;
}

.weather-history a:hover {
  color: #0056b3;
}

.weather-history button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 20px;
}

.weather-history button:hover {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #c82333;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>