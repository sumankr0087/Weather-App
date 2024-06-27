<template>
    <div>
        <div v-if="dailyForecast.length" class="bg-gray-600 bg-opacity-15 backdrop-blur-lg px-6 py-4 rounded-lg">
            <div class="flex gap-4 justify-between">
                <div class="text-white text-xs font-normal" v-for="(forecast, index) in dailyForecast" :key="index">
                    <p class="mb-2">{{ forecast.date }}</p>
                    <p class="mb-2">{{ forecast.temp_max }} °C</p>
                    <p class="mb-2">{{ mapWeatherDescription(forecast.description) }}</p>
                </div>
            </div>
        </div>
        <div class="mt-8 text-base font-medium text-[#fff] gap-4">
            <h1 class="mb-2">Description</h1>
            <p class="text-xs font-medium text-gray-100">
                To summarize and brief in short, Weather App is the application of science and technology to predict the
                conditions of the atmosphere for a given location and time.
            </p>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "DailyForecast",
    props: ["dailyForecast"],
    data() {
        return {
            dailyForecastData: this.dailyForecast,
        };
    },
    methods: {
        mapWeatherDescription(description) {
            const weatherMap = {
                "clear sky": "Sunny",
                "few clouds": "Partly Cloudy",
                "scattered clouds": "Cloudy",
                "broken clouds": "Cloudy",
                "overcast clouds": "Cloudy",
                "light rain": "Rain",
                "moderate rain": "Rain",
                "heavy intensity rain": "Rain",
                "thunderstorm with rain": "Thunderstorm",
            };
            return weatherMap[description.toLowerCase()] || description;
        }
    },
};
</script>
