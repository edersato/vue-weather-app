<template>
  <div class="weather-wrap" v-if="weatherAvailable">
    <div class="location-box">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="date">{{ dateBuilder() }}</div>
    </div>

    <div class="weather-box">
      <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
      <div class="weather-desc">{{ weather.weather[0].description }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeatherDisplay",
  props: {
    weather: Object
  },
  methods: {
    dateBuilder() {
      let d = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let days = ["Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  },
  computed: {
    weatherAvailable() {
      return typeof this.weather.main !== 'undefined';
    }
  }
};
</script>
