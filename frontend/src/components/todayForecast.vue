<template>
    <div>
        <div v-if="localWeatherData"
            class="text-[#fff] justify-center grid items-center mb-[3rem] p-8 bg-[#F6CE7AD6] rounded-2xl">
            <div class="flex justify-center text-xl font-medium gap-2 mb-2 text-[#ff7e53b5]">
                Today <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor" class="size-5 flex mt-[6px]">
                    <path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5" />
                </svg>
            </div>
            <div class="flex gap-4 text-[#ff7e53b5] font-medium mb-2">
                <template v-if="weatherCondition === 'Sunny'">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="size-24">
                        <path
                            d="M8 1a.75.75 0 0 1 .75.75v1.5a.75.75 0 0 1-1.5 0v-1.5A.75.75 0 0 1 8 1ZM10.5 8a2.5 2.5 0 1 1-5 0 2.5 2.5 0 0 1 5 0ZM12.95 4.11a.75.75 0 1 0-1.06-1.06l-1.062 1.06a.75.75 0 0 0 1.061 1.062l1.06-1.061ZM15 8a.75.75 0 0 1-.75.75h-1.5a.75.75 0 0 1 0-1.5h1.5A.75.75 0 0 1 15 8ZM11.89 12.95a.75.75 0 0 0 1.06-1.06l-1.06-1.062a.75.75 0 0 0-1.062 1.061l1.061 1.06ZM8 12a.75.75 0 0 1 .75.75v1.5a.75.75 0 0 1-1.5 0v-1.5A.75.75 0 0 1 8 12ZM5.172 11.89a.75.75 0 0 0-1.061-1.062L3.05 11.89a.75.75 0 1 0 1.06 1.06l1.06-1.06ZM4 8a.75.75 0 0 1-.75.75h-1.5a.75.75 0 0 1 0-1.5h1.5A.75.75 0 0 1 4 8ZM4.11 5.172A.75.75 0 0 0 5.173 4.11L4.11 3.05a.75.75 0 1 0-1.06 1.06l1.06 1.06Z" />
                    </svg>
                </template>
                <template v-else-if="weatherCondition === 'Cloudy'">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="size-24">
                        <path
                            d="M1 9.5A3.5 3.5 0 0 0 4.5 13H12a3 3 0 0 0 .917-5.857 2.503 2.503 0 0 0-3.198-3.019 3.5 3.5 0 0 0-6.628 2.171A3.5 3.5 0 0 0 1 9.5Z" />
                    </svg>
                </template>

                <p class="text-5xl mt-6">{{ temperature }} °C</p>
            </div>
            <div class="mb-2 flex text-xl font-medium justify-center text-[#ff7e53b5]">{{ weatherCondition }}</div>

            <div class="text-base font-medium flex justify-center mb-2 text-[#ff7e53b5]">
                {{ localWeatherData.name }},
                {{ localWeatherData.sys.country }}
            </div>

            <div class="text-base font-medium flex justify-center mb-2 text-[#ff7e53b5]">
                {{ formattedCurrentDate }}
            </div>

            <div class="text-base font-medium flex justify-center mb-2 text-[#ff7e53b5]">
                Feels Like {{ feelsLikeTemperature }} | Sunset {{ formattedSunset }}
            </div>
        </div>
    </div>
</template>

  
<script>
export default {
    name: "TodayForecast",
    props: ["weatherData"],
    data() {
        return {
            localWeatherData: this.weatherData
        };
    },
    computed: {
        temperature() {
            return this.localWeatherData
                ? Math.floor(this.localWeatherData.main.temp - 273)
                : null;
        },
        feelsLikeTemperature() {
            return this.localWeatherData
                ? Math.floor(this.localWeatherData.main.feels_like - 273)
                : null;
        },
        iconUrl() {
            return this.localWeatherData
                ? `http://openweathermap.org/img/w/${this.localWeatherData.weather[0].icon}.png`
                : null;
        },
        weatherCondition() {
            if (!this.localWeatherData) return null;

            const description = this.localWeatherData.weather[0].description.toLowerCase();
            if (description.includes("sun")) return "Sunny";
            if (description.includes("cloud")) return "Cloudy";
            if (description.includes("rain")) return "Rain";
            if (description.includes("snow")) return "Snow";
            if (description.includes("thunderstorm")) return "Thunderstorm";
            if (description.includes("mist") || description.includes("fog")) return "Mist/Fog";

            return description.charAt(0).toUpperCase() + description.slice(1);
        },
        formattedSunrise() {
            if (!this.localWeatherData) return null;

            const date = new Date(this.localWeatherData.sys.sunrise * 1000);
            return date.toLocaleTimeString(undefined, { hour: '2-digit', minute: '2-digit' });
        },
        formattedSunset() {
            if (!this.localWeatherData) return null;

            const date = new Date(this.localWeatherData.sys.sunset * 1000);
            return date.toLocaleTimeString(undefined, { hour: '2-digit', minute: '2-digit' });
        },
        formattedCurrentDate() {
            const months = [
                "Jan", "Feb", "Mar", "Apr", "May", "Jun",
                "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
            ];
            const today = new Date();
            const day = today.getDate();
            const month = months[today.getMonth()];
            const year = today.getFullYear();
            return `${day} ${month} ${year}`;
        }
    }
};
</script>
