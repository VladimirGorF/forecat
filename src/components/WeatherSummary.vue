<script setup>
// импортируем функцию Загл буквы из utils
import { capitalaiseFirstLetter } from "../utils/index";
// регистрируем пропс забинденный в верстке в App в тэге WeatherSummary
const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true,
  },
});
const today = new Date().toLocaleString("en-EN", {
  weekday: "short",
  year: "numeric",
  month: "long",
  day: "numeric",
});
</script>

<template>
  <div class="summary">
    <div
      :style="`
        background-image: url('/weather-main/${weatherInfo?.weather[0].description}.png');`"
      class="pic-main"
    ></div>
    <div class="weather">
      <div class="temp">{{ Math.round(weatherInfo?.main?.temp) }}</div>
      <div class="weather-desc text-block">
        {{ capitalaiseFirstLetter(weatherInfo?.weather[0].description) }}
      </div>
    </div>
    <div class="city text-block">
      {{ weatherInfo?.name }}, {{ weatherInfo?.sys?.country }}
    </div>
    <div class="date text-block">{{ today }}</div>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main.sass'
@import '../assets/styles/weatherSummary.sass'
</style>
