<template>
  <div class="search-box">
    <input
      type="text"
      class="search-bar"
      placeholder="Busque uma cidade"
      v-model="query"
      @keypress.enter="fetchWeather(); fetchDaily();"
      />
      <!-- @keypress.enter="fetchWeather();" -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      //OpenWeather
      api_key: "17cbaaac382b9f057f5d10130dd982b8",
      url_base: "https://api.openweathermap.org/data/2.5/",
      lang: "pt_br",
      units: "metric",
      query: "",

      //WeatherApi
      apiForecast: "35b23f032406442889d125018250302",
      urlForecast: "http://api.weatherapi.com/v1/forecast.json",
    };
  },

  methods: {
    async fetchWeather() {
      try {
        const current = await axios.get(
          `${this.url_base}weather?q=${this.query}&lang=${this.lang}&units=metric&APPID=${this.api_key}`
        );
        this.$emit("weather-fetched", current.data);
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },

    async fetchDaily() {
      try {
        const daily = await axios.get(
          `${this.urlForecast}?key=${this.apiForecast}&q=${this.query}&days=7&aqi=no&alerts=no`  
        );
        this.$emit("forecast-fetched", daily.data);
      }
      catch(error) {
        console.error(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.search-box {
  display: flex;
  justify-content: center;
  margin-top: 2em;

  .search-bar {
    display: block;
    padding: 1.5rem;
    width: 90%;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 8px #00000040;
    background-color: #ffffff66;
    border-radius: 0 16px;
    transition: 0.4s;
    text-align: center;

    &:focus {
      box-shadow: 0px 0px 16px #00000040;
      background-color: #ffffff66;
      border-radius: 16px 0px;
      text-align: center;
      font-size: 1.3rem;
    }

    &::placeholder {
      text-align: center;
    }
  }
}
</style>