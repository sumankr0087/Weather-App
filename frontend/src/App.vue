<template>
  <div class="px-8 py-12 gap-8 grid md:grid-cols-2 grid-cols-1">
    <TodayForecast :weatherData="weatherData" />
    <DailyForecast :dailyForecast="dailyForecast" />
  </div>
</template>

<script>
import axios from "axios";
import './style.css'
import TodayForecast from "./components/todayForecast.vue"
import DailyForecast from "./components/dailyForecast.vue"

const apikey = "64acce97067bba5afa8645bb8ac84a05";

export default {
  name: "App",
  components: { TodayForecast, DailyForecast },
  data() {
    return {
      city: "jharkhand",
      weatherData: null,
      dailyForecast: [],
    };
  },
  mounted() {
    this.we
    this.fetchCurrentLocationWeather();
  },
  methods: {
    async fetchCurrentLocationWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apikey}`;
      await this.fetchWeatherData(url);
    },
    async fetchWeatherData(url) {
      try {
        const response = await axios.get(url);
        this.weatherData = response.data;
        await this.fetchForecast(this.weatherData.name);
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
    async fetchForecast() {
      const urlcast = `https://api.openweathermap.org/data/2.5/forecast?q=${this.city}&appid=${apikey}`;
      try {
        const response = await axios.get(urlcast);
        const forecast = response.data;
        this.dayForecast(forecast);
      } catch (error) {
        console.error("Error fetching forecast data:", error);
      }
    },
    dayForecast(forecast) {
      this.dailyForecast = [];
      const options = { day: 'numeric', month: 'short', year: 'numeric' }; // Date formatting options

      for (let i = 8; i < forecast.list.length; i += 8) {
        const date = new Date(forecast.list[i].dt * 1000);
        const formattedDate = date.toLocaleDateString('en-GB', options); // Format the date
        this.dailyForecast.push({
          date: formattedDate,
          temp_max: Math.floor(forecast.list[i].main.temp_max - 273),
          description: forecast.list[i].weather[0].description,
        });
      }
    },

  },
};
</script>