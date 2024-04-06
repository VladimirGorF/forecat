<script setup>
import { ref, onMounted, computed } from "vue";
import { capitalaiseFirstLetter } from "./utils/index";
import { API_KEY, BASE_URL } from "./constants/index.js";
import WeatherSummary from "./components/WeatherSummary.vue";
import HiLights from "./components/HiLights.vue";
import Cords from "./components/Cords.vue";
import Humidity from "./components/Humidity.vue";

const isError = computed(() => weatherInfo.value?.cod !== 200);

const city = ref("Moscow"); // локал storage нужен для определения последнего введенного города пользователем
const weatherInfo = ref(null);

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => (weatherInfo.value = data));
}
onMounted(getWeather); // при монтировании вызываем сразу получение данных о погоде

weatherInfo.weather = computed(() => {
  if (props.weatherInfo?.cod == 404) {
    return [{ id: 0, main: "NotFound", description: "NotFound", icon: "04d" }];
  }
});
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section
              :class="['section', 'section-left', { 'section-error': isError }]"
            >
              <div class="info">
                <div class="city-inner">
                  <input
                    v-model="city"
                    type="text"
                    class="search"
                    @keyup.enter="getWeather"
                  />
                </div>
                <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo" />
                <div v-else class="error">
                  <div class="error-title">Ooops! Somthing went wrong!</div>
                  <div v-if="weatherInfo?.message" class="error-message"> {{capitalaiseFirstLetter(weatherInfo?.message)}}</div>
                </div>
              </div>
            </section>
            <section v-if="!isError" class="section section-right">
              <HiLights :weatherInfo="weatherInfo" />
            </section>
          </div>
          <div v-if="!isError" class="sections">
            <Cords :coord="weatherInfo.coord" />
            <Humidity :humidity="weatherInfo.main.humidity" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="sass" scoped>
@import './assets/styles/main.sass'
@import './assets/styles/app.sass'
</style>
