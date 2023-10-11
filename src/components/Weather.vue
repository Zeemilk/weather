<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import moment from 'moment'

const weatherData = ref({})
const forecastData = ref({})
const key = ref('b5b28f173cf34f5033d39a7b3bdb322c')
const lon = ref(13)
const long = ref(100)

const url1 = ref(`https://api.openweathermap.org/data/2.5/weather?lat=${lon.value}&lon=${long.value}&appid=${key.value}`)
const url2 = ref(`https://api.openweathermap.org/data/2.5/forecast?lat=${lon.value}&lon=${long.value}&appid=${key.value}`)

function weather1() {
  axios
    .get(url1.value)
    .then((response) => {
      weatherData.value = response.data
      console.log(weatherData.value)
    })
    .catch((err) => {
      console.error(err)
    })
}

function weather2() {
  axios
    .get(url2.value)
    .then((response) => {
      forecastData.value = response.data
    })
    .catch((err) => {
      console.error(err)
    })
}

function formatTimestamp(x) {
  return moment.unix(x).format('YYYY-MM-DD HH:mm:ss');
}

onMounted(() => {
  weather1(),
  weather2()
})
</script>

<template>
  <div class="weather-app">
    <div class="current-weather" v-if="weatherData.main">
      <h2>Current Weather</h2>
      <div class="weather-info">
        <p>Temperature: {{ (weatherData.main.temp - 273.15).toFixed(2) }}°C</p>
        <p>Pressure: {{ weatherData.main.pressure }} hPa</p>
        <p>Timestamp: {{ formatTimestamp(weatherData.dt) }}</p>
      </div>
    </div>
  </div>
  <br /><br />
  <div class="weather-app">
    <div class="forecast" v-if="forecastData.list">
      <h2>Forecast</h2>
      <div class="forecast-grid">
        <div class="forecast-item" v-for="(day, index) in forecastData.list" :key="index">
          <div class="forecast-info">
            <p>อนาคตอีก {{ index * 3 + 1 }} ชั่วโมง</p>
            <p>Temperature: {{ (day.main.temp - 273.15).toFixed(2) }}°C</p>
            <p>Pressure: {{ day.main.pressure }} hPa</p>
            <p>Timestamp: {{ formatTimestamp(day.dt) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
.weather-app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  background-color: blueviolet;
}

.current-weather,
.forecast {
  margin: 20px 0;
}

.weather-info,
.forecast-info {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  margin: 10px 0;
}

.forecast-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr); 
  gap: 20px;
}
</style>
