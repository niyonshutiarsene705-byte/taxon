<template>
  <div>
    <div style="display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap;">
      <input
        v-model="city"
        @keyup.enter="handleSearch"
        type="text"
        placeholder="Enter city (London)"
        :disabled="loading"
      />
      <button @click="handleSearch" :disabled="loading">
        {{ loading ? 'Loading...' : 'Search' }}
      </button>
      <button @click="handleGeolocation" :disabled="loading">Location</button>
    </div>

    <div v-if="loading">Loading...</div>

    <div v-if="errorMsg" style="color: red;">{{ errorMsg }}</div>

    <div v-if="weatherData" style="background: #111; padding: 20px; border: 2px solid red; border-radius: 10px;">
      <h2>{{ weatherData.name }}</h2>
      <p>Temperature: {{ Math.round(weatherData.main.temp) }}°C {{ getWeatherIcon(weatherData.weather[0].main) }}</p>
      <p>Condition: {{ weatherData.weather[0].description }}</p>
      <p>Humidity: {{ weatherData.main.humidity }}%</p>
      <p>Wind: {{ weatherData.wind.speed }} m/s</p>
      <p>Feels like: {{ Math.round(weatherData.main.feels_like) }}°C</p>
    </div>

    <p v-if="!loading && !weatherData && !errorMsg" style="color: white;">Enter city to search</p>
    <p v-if="noApiKey" style="color: red;">No API key! Add VITE_API_KEY to .env</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const city = ref('')
const weatherData = ref(null)
const loading = ref(false)
const errorMsg = ref('')
const noApiKey = ref(false)

const apiKey = import.meta.env.VITE_API_KEY

if (!apiKey) {
  noApiKey.value = true
}

async function fetchWeather(query, useCoords = false) {
  if (!apiKey) {
    errorMsg.value = 'No API key in .env'
    return
  }
  loading.value = true
  errorMsg.value = ''
  try {
    let url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(query)}&appid=${apiKey}&units=metric`
    if (useCoords) {
      const [lat, lon] = query
      url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`
    }
    const res = await fetch(url)
    if (!res.ok) {
      const err = await res.text()
      throw new Error(err || 'City not found')
    }
    weatherData.value = await res.json()
    if (!useCoords) {
      localStorage.setItem('lastCity', query)
      city.value = ''
    }
  } catch (err) {
    console.error('Weather error:', err)
    errorMsg.value = 'City not found - check name'
    weatherData.value = null
  }
  loading.value = false
}

const handleSearch = () => fetchWeather(city.value.trim())

const handleGeolocation = () => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      pos => fetchWeather([pos.coords.latitude, pos.coords.longitude], true),
      () => errorMsg.value = 'Location denied'
    )
  }
}

const getWeatherIcon = main => main === 'Clear' ? '☀️' : main === 'Clouds' ? '☁️' : '🌧️' ? '🌧️' : '🌤️'

onMounted(async () => {
  const last = localStorage.getItem('lastCity')
  if (last) await fetchWeather(last)
})
</script>

