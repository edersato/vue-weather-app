<template>
  <div :class="weatherClass">
    <h1>{{ title }}</h1>
    <SearchBar @weather-fetched="setWeather" />
    <CityInfo :weather="weather" />
  </div>
</template>

<script>
import CityInfo from "../components/CityInfo.vue";
import SearchBar from "../components/SearchBar.vue";

export default {
  components: {
    SearchBar,
    CityInfo,
  },

  data() {
    return {
      title: "Weather App",
      weather: {},
    };
  },

  computed: {
    weatherClass() {
      return typeof this.weather.main !== "undefined" &&
        this.weather.main.temp > 25
        ? "hot"
        : "";
    },
  },

  methods: {
    setWeather(weatherData) {
      this.weather = weatherData;
    },
  },
};
</script>

<style>
h1,
h2 {
  text-align: center;
  padding-top: 1em;
  color: white;
}
</style>