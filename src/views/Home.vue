<template>
  <div id="home" :class="weatherClass">
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

<style lang="scss" scoped>
#home {
  transition: 0.6s;
  background: linear-gradient(#85c1e9, #d4d4d4);
}

#home.hot {
  transition: 0.6s;
  background: linear-gradient(#ffa500, #d4d4d4);
}
</style>