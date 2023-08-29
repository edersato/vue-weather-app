<template>
  <div class="search-box">
    <input
      type="text"
      class="search-bar"
      placeholder="Busque uma cidade"
      v-model="query"
      @keypress.enter="fetchWeather"
    />
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      api_key: "17cbaaac382b9f057f5d10130dd982b8",
      url_base: "https://api.openweathermap.org/data/2.5/",
      lang: "pt_br",
      query: "",
    };
  },

  methods: {
    async fetchWeather() {
      try {
        const response = await axios.get(
          `${this.url_base}weather?q=${this.query}&lang=${this.lang}&units=metric&APPID=${this.api_key}`
        );
        this.$emit("weather-fetched", response.data);
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
  },
};
</script>

<style>
</style>