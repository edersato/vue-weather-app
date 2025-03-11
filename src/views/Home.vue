<template>
  <div id="home" :class="weatherClass">
    <h1>{{ title }}</h1>
    <SearchBar @weather-fetched="setWeather" @forecast-fetched="setForecast" />
    <CityInfo :weather="weather" :forecast="forecast" />
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
      forecast: {},
    };
  },

  computed: {
    weatherClass() {
      if (typeof this.weather.main !== "undefined" && this.weather.main.temp > 25) {
        return "hot";
      } else if (typeof this.weather.main !== "undefined" && this.weather.main.temp < 25) {
        return "cold";
      } else {
        return "home";
      }
    },
  },

  methods: {
    setWeather(data) {
      this.weather = data;
    },
    
    setForecast(data) {
      this.forecast = data;
    }
  },
};
</script>

<style lang="scss" scoped>
#home {
  background-color: #d2d2d2;
}

#home.cold {
  transition: 0.6s;
  background: linear-gradient(#85c1e9, #d4d4d4, #d1d1d1);
}

#home.hot {
  transition: 0.6s;
  background: linear-gradient(#ffa500, #d4d4d4, #d1d1d1);
}
</style>